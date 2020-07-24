# network-survey-messaging
Defines a set of radio frequency (RF) related message schemas in JSON format.

Network Survey Messaging
===============

**Readme Contents**
- [What is it?](#what-is-it?)
- [Why?](#why?)
- [Some Details](#some-details)
- [Why are there protobuf files?](#why-are-there-protobuf-files?)
- [Build and development instructions](#build-and-development-instructions)
- [Change log](#change-log)
- [Contact](#contact)


## What is it?
The Network Survey Messaging API defines a set of messages that can be used for sending wireless network survey 
messages between two applications.

## Why?
The original purpose for this API was to facilitate sending LTE survey messages from the [Network Survey Android App](https://github.com/christianrowlands/android-network-survey)
to a remote server via gRPC. However, it has been expanded not only to support additional protocols such as
GSM, CDMA, UMTS and 802.11, but has also been expanded to allow for its use outside of the original Android application
by any system wanting to share wireless protocol data, or send wireless signal events.


## Some Details
The messages in this API are defined using [AsyncAPI](https://www.asyncapi.com/). All the messages are in JSON format. 
In addition to the messages this API defines, it also defines the topics the messages can be sent on. For the most part,
the topic simply follows the message name, but the idea is that these messages can be sent over transports such as MQTT
or Kafka. If they are, then the topic name in MQTT, Kafka, or any other system should follow the "channel" defined in
this API. For example, the [Network Survey Android App](https://github.com/christianrowlands/android-network-survey) 
has a feature that sends out the GSM, CDMA, UMTS, LTE, and 802.11 JSON messages defined in this API. In doing so, it 
publishes them to the MQTT topic that follows the channel name in this API. Therefore, anyone wanting to consume survey
records from that app simply needs to subscribe the the pre-defined topic/channel name and consume the JSON messages.


## Why are there protobuf files?
Protobuf was originally used at the IDL, and gRPC was used as the RPC framework / transport. However, to allow the 
survey messages to flow over tools such as Kafka and be consumed more easily without requiring a protobuf library to 
deserialize the binary messages, the actual messages were sent in JSON instead. That being said, it is really easy to
construct a protobuf object and use the Protobuf `JsonFormat.Printer` to convert a protobuf object to JSON. Therefore,
proto definitions matching the JSON messages are provided as a convenient way to generate the JSON message strings.

In other words, the protobuf files and gRPC SDKs are provided as convenience libraries only and are not an official part
of the Network Survey Messaging API. The API is instead only the JSON message schema and channel/topic names for the messages.

As a result, if a gRPC setup is of interest to a user of these messages, then the generated library can be employed to
send the protobuf messages over gRPC.  The build produces a Java library with all of the messages for use by the client 
and server software respectively.

More information about protobuf is available [here](https://developers.google.com/protocol-buffers/).
More information about gRPC is available [here](https://grpc.io/).


## Sample usage
The gRPC website has tutorials for utilizing gRPC + protobuf for [Android](https://grpc.io/docs/quickstart/android/) and
[Java](https://grpc.io/docs/quickstart/java/).


## Getting Started
#### Add Network Survey Messaging to your project

The Network Survey Messaging protobuf library is available via [mavenCentral](https://search.maven.org/search?q=network-survey-messaging)

```groovy
dependencies {
    implementation 'com.craxiom:network-survey-messaging:0.1.3'
}
```

## Build and development instructions
#### Prerequisites

To build the the AsyncAPI files you need to have `npm` installed. For macOS, it can be installed via brew
 - `brew install node`
 
Then install the html template generator
 - `npm install -g @asyncapi/html-template`
 
Additional AsyncAPI generators can be found here:  https://github.com/asyncapi/generator#list-of-official-generator-templates

#### Building the HTML content from the AsyncAPI file
 - Execute `ag src/main/asyncapi/network_survey_messaging.yaml @asyncapi/html-template -o build/network-survey-messaging-html`
 - The HTML content will be located in the directory specified after the `-o` option.
 
To publish a new version of the Network Survey Messaging API HTML page, use the following command
 - `ag src/main/asyncapi/network_survey_messaging.yaml @asyncapi/html-template -o docs`

This will overwrite the current HTML content from the docs directory with the last API definition from the yaml file.
 
#### Building the protobuf Java library
 - Execute `gradlew build` in the root directory to produce the Java library with the protobuf messages.
 - Execute `gradlew publishToMavenLocal` in the root directory to publish the artifacts to the local Maven cache.


##Change log
##### [0.1.3](https://github.com/christianrowlands/network-survey-messaging/releases/tag/v0.1.3) - 2020-06-26
 * Added the device_name field to all the wireless proto messages.
 
##### [0.1.2](https://github.com/christianrowlands/network-survey-messaging/releases/tag/v0.1.2) - 2020-05-29
 * Swapped out the 802.11 service set type from a String to an enum.

##### [0.1.1](https://github.com/christianrowlands/network-survey-messaging/releases/tag/v0.1.1) - 2020-05-23
 * Added a protobuf message for 802.11 beacon frames.

##### [0.1.0](https://github.com/christianrowlands/network-survey-messaging/releases/tag/v0.1.0) - 2020-04-23
 * Switched to the full java version instead of the java lite protobuf implementation.

##### [0.0.2](https://github.com/christianrowlands/network-survey-messaging/releases/tag/release-0.0.2) - 2020-01-06
 * Added support for streaming GSM, CDMA, and UMTS cellular survey records.
 
##### [0.0.1](https://github.com/christianrowlands/network-survey-messaging/releases/tag/release-0.0.1) - 2019-09-27
 * Initial release of message definitions.


Contact
-----------------------------------
Christian Rowlands <craxiomdev@gmail.com> 