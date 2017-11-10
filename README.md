# OSCMessenger

The purpose of OSCMessenger is to create a manufacturer/developer agnostic chat room that will allow users to communicate between devices and applications within an IP network.

This document describe the integration methods of OSCMessenger via the Open Sound Control protocol (OSC).

The method for transmitting and receiving OSC packets is over UDP via the multicast IP address 239.254.0.1 on Port 3090.

All examples in this document will use the following format:
<OSC Address Pattern> = <OSC Argument 1>, <OSC Argument 2>, <OSC Argument 3> (etc…)

OSC Arguments will be specified in one of the following formats:
<OSC Argument Type: Description>
<OSC Argument Type: Example>

## OSC UUID:

UUID’s uniquely identify each messenger client and message, and should be preserved within each client’s database. This allows you to uniquely identify who the message is coming from as well as which message you are referring to.

UID’s will be specified as strings in the following format:
XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX

Ex:
“B0BAE0A0-3BBE-4004-888B-F61CA125D0B0”

## Integrating Your App: Step 1 – Introductions:

After joining the multicast group 239.254.0.1 and listening to OSC messages on port 3090.
Introduce yourself:
- /messenger/me/introduce = <string: client OSC UID>, <string: user name>, <string: application name>

All clients will reply with:
- /messenger/me/welcome = <string: client OSC UID>, <string: user name>, <string: application name>


## Integrating Your App: Step 2 – Rooms:

When entering a room:
- /messenger/me/room/<string: room name>/enter= <string: client OSC UID>, <string: user name>, <string: application name>

When exiting a room:
- /messenger/me/room/<string: room name>/exit = <string: client OSC UID>, <string: user name>, <string: application name>

To query who is listening outside of a room:
- /messenger/who/listening = <string: room name>

All clients listening outside of a room will reply with:
- /messenger/me/listening = <string: client OSC UID>, <string: user name>, <string: application name>

To query who is listening within a room:
- /messenger/who/listening/room = <string: room name>

All clients listening within the room will reply with:
- /messenger/me/listening/room/<string: room name> = <string: client OSC UID>, <string: user name>, <string: application name>


## Integrating Your App: Step 3 – Sending a message:

Sending a message outside of a room:
- /messenger/me/say = <string: message OSC UID> <string: client OSC UID>, <string: message>

Send a message within a room:
- /messenger/me/room/<room name>/say = <string: message OSC UID> <string: client OSC UID>, <string: message>


## Integrating Your App: Step 4 – Farewell:

On a client’s departure from messenger, they should declare they are leaving with:
- /messenger/me/farewell = <string: client OSC UID>, <string: user name>, <string: application name>

