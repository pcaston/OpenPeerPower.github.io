---
title: MySensors
description: Instructions on how to integrate MySensors sensors into Open Peer Power.
ha_category:
  - DIY
ha_iot_class: Local Push
ha_release: 0.73
ha_codeowners:
  - '@MartinHjelmare'
ha_domain: mysensors
excerpt: none
---

The [MySensors](https://www.mysensors.org) project combines devices like Arduino, ESP8266, Raspberry Pi, NRF24L01+ and RFM69 to build affordable sensor networks. This integration will automatically add all available devices to Open Peer Power, after [presentation](#presentation) is done. That is, you do not need to add anything to your configuration for the devices for them to be added. Go to the **states** section of the developer tools to find the devices that have been identified.

## Configuration

Integrate your Serial, Ethernet (LAN) or MQTT MySensors Gateway by adding the following to your `configuration.yaml` file:

{% highlight yaml %}
# Example configuration.yaml entry
mysensors:
  gateways:
    - device: '/dev/ttyUSB0'
{% endhighlight %}

<div class='note'>
Not all features of MySensors 2.x are supported by Open Peer Power yet. As more features are added, they will be described here in the documentation. Go to the MySensors platform pages under "related components" to see what message types are currently supported.
</div>

If you are using an original Arduino as a serial gateway, the port will be named `ttyACM*`. The exact number can be determined with the command shown below.

{% highlight bash %}
ls /dev/ttyACM*
{% endhighlight %}

If you are using the MQTT gateway, you also need to have the [MQTT component](/integrations/mqtt/) configured in Open Peer Power. See below for a minimum MQTT configuration:

{% highlight yaml %}
mqtt:
  client_id: home-assistant-1
{% endhighlight %}

<div class='note'>
The MQTT gateway requires MySensors version 2.0+ and only the MQTT client gateway is supported.
</div>

### Extended configuration example

{% highlight yaml %}
# Example configuration.yaml entry
mysensors:
  gateways:
    - device: '/dev/ttyUSB0'
      persistence_file: 'path/mysensors.json'
      baud_rate: 38400
      nodes:
        1:
          name: 'kitchen'
        3:
          name: 'living_room'
    - device: '/dev/ttyACM0'
      persistence_file: 'path/mysensors2.json'
      baud_rate: 115200
    - device: '192.168.1.18'
      persistence_file: 'path/mysensors3.json'
      tcp_port: 5003
    - device: mqtt
      persistence_file: 'path/mysensors4.json'
      topic_in_prefix: 'mygateway1-out'
      topic_out_prefix: 'mygateway1-in'
  optimistic: false
  persistence: true
  retain: true
  version: '2.0'
{% endhighlight %}

### Presentation

Present a MySensors sensor or actuator, by following these steps:

1. Connect the serial gateway to your computer or the Ethernet or MQTT gateway to your network.
2. Configure the MySensors integration in `configuration.yaml`.
3. Start Open Peer Power.
4. Write and upload your MySensors sketch to the sensor. Make sure you:
    - Send sketch name.
    - Present the sensor's `S_TYPE`.
    - Send at least one initial value per `V_TYPE`. In version 2.x of MySensors, this has to be done in the loop function. See below for an example in 2.0 of how to make sure the initial value has been received by the controller.
5. Start the sensor.

{% highlight cpp %}
/*
 * Documentation: https://www.mysensors.org
 * Support Forum: https://forum.mysensors.org
 *
 * https://www.mysensors.org/build/relay
 */

#define MY_DEBUG
#define MY_RADIO_NRF24
#define MY_REPEATER_FEATURE
#define MY_NODE_ID 1
#include <SPI.h>
#include <MySensors.h>
#include <Bounce2.h>

#define RELAY_PIN  5
#define BUTTON_PIN  3
#define CHILD_ID 1
#define RELAY_ON 1
#define RELAY_OFF 0

Bounce debouncer = Bounce();
bool state = false;
bool initialValueSent = false;

MyMessage msg(CHILD_ID, V_STATUS);

void setup()
{
  pinMode(BUTTON_PIN, INPUT_PULLUP);
  debouncer.attach(BUTTON_PIN);
  debouncer.interval(10);

  // Make sure relays are off when starting up
  digitalWrite(RELAY_PIN, RELAY_OFF);
  pinMode(RELAY_PIN, OUTPUT);
}

void presentation()  {
  sendSketchInfo("Relay+button", "1.0");
  present(CHILD_ID, S_BINARY);
}

void loop()
{
  if (!initialValueSent) {
    Serial.println("Sending initial value");
    send(msg.set(state?RELAY_ON:RELAY_OFF));
    Serial.println("Requesting initial value from controller");
    request(CHILD_ID, V_STATUS);
    wait(2000, C_SET, V_STATUS);
  }
  if (debouncer.update()) {
    if (debouncer.read()==LOW) {
      state = !state;
      // Send new state and request ack back
      send(msg.set(state?RELAY_ON:RELAY_OFF), true);
    }
  }
}

void receive(const MyMessage &message) {
  if (message.isAck()) {
     Serial.println("This is an ack from gateway");
  }

  if (message.type == V_STATUS) {
    if (!initialValueSent) {
      Serial.println("Receiving initial value from controller");
      initialValueSent = true;
    }
    // Change relay state
    state = (bool)message.getInt();
    digitalWrite(RELAY_PIN, state?RELAY_ON:RELAY_OFF);
    send(msg.set(state?RELAY_ON:RELAY_OFF));
  }
}
{% endhighlight %}

### SmartSleep

Sending a heartbeat, `I_HEARTBEAT_RESPONSE`, from the MySensors device to Open Peer Power, using MySensors version 2.0 - 2.1, activates the SmartSleep functionality in Open Peer Power. This means that messages are buffered and only sent to the device upon receiving a heartbeat from the device. State changes are stored so that only the last requested state change is sent to the device. Other types of messages are queued in a FIFO queue. SmartSleep is useful for battery powered actuators that are waiting for commands. See the MySensors library API for information on how to send heartbeats and sleep the device.

In MySensors version 2.2 the serial API changed from using `I_HEARTBEAT_RESPONSE` to signal SmartSleep, to using `I_PRE_SLEEP_NOTIFICATION` and `I_POST_SLEEP_NOTIFICATION`. Open Peer Power has been upgraded to support the new message types and will activate SmartSleep when receiving a message of type `I_PRE_SLEEP_NOTIFICATION`, if using MySensors version 2.2.x or higher. If Open Peer Power is configured to use MySensors version 2.0 - 2.1 the old SmartSleep behavior is retained.

### Message validation

Messages sent to or from Open Peer Power from or to a MySensors device will be validated according to the MySensors [serial API](https://www.mysensors.org/download/serial_api_20). If a message doesn't pass validation, it will be dropped and not be passed forward either to or from Open Peer Power. Make sure you follow the serial API for your version of MySensors when writing your Arduino sketch.

The log should warn you of messages that failed validation or if a child value is missing that is required for a certain child type. Open Peer Power will log failed validations of child values at warning level if e.g.,  one required value type for a platform has been received, but other required value types are missing.

Message validation was introduced in version 0.52 of Open Peer Power.

### Debug logging

If you experience dropped messages or that a device is not added to Open Peer Power, please turn on debug logging for the `mysensors` integration and the `mysensors` package. This will help you see what is going on. Make sure you use these logging settings to collect a log sample if you report an issue about the `mysensors` integration in our GitHub issue tracker.

{% highlight yaml %}
logger:
  default: info
  logs:
    homeassistant.components.mysensors: debug
    mysensors: debug
{% endhighlight %}

Visit the [library API][MySensors library api] of MySensors for more information.

[MySensors library API]: https://www.mysensors.org/download
