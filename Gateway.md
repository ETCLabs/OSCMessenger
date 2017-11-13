# Gateway: Optional Implementation

In most instances of OSC Messenger multicasting as a medium of distributing messages works out well. One shortcoming though is that routing multicast packets comes at great financial expense. To alleviate the pain we offer a solution where by a client can be a gateway and forward between unicast and multicast in both directions. 
All gateway messages are sent via a unicast UDP or TCP channel. TCP is recommended. 
All clients acting as OSC Messenger gateways, otherwise known as “Gateways”, should receive all unicast messages on port 9030.

Any client may act as an OSC Messenger gateway for a number of unicasting Clients, known as “Patrons” and receive gateway messages on an ephemeral port.


## Integrating Your App with a Gateway: Step 1 – Request a Gateway:

The patron will request another client to be a gateway for it via a unicast channel:
- /messenger/gateway/me/request = <string: patron OSC UID>

The proposed gateway will reply via the unicast channel with one of the following:

If the proposed gateway is able to act as a Gateway for this unicast patron, it must inform the Patron with:
- /messenger/gateway/me/accept = <string: gateway OSC UID>, <string: patron OSC UID>

If the proposed gateway is not able to act as a Gateway for this unicast patron (e.g. it is at capacity or does not offer gateway services), it may inform the Patron with:
- /messenger/gateway/me/deny = <string: gateway OSC UID>, <string: patron OSC UID>

Clients that are unable to offer the gateway service may also choose not to reply.
If no reply is received within 5 seconds a Patron should assume the Client is not currently able to be a Gateway.


##  Integrating Your App with a Gateway: Step 2 – Gateway Forwarding:

The Gateway shall act as follows:
- All OSC Messenger methods except for ”/messenger/gateway/..." received by the Gateway from the multicast group are forwarded via the unicast channel(s) to all its Patron(s).
- All OSC Messenger methods except for ”/messenger/gateway/..."received by the Gateway from connected Patrons are sent to the multicast group and forwarded to all other connected Patrons.
As far as all other OSC Messenger applications are concerned, the Gateway Client will simply appear to have multiple users or multiple running messenger applications.


##  Integrating Your App with a Gateway: Step 3 – Keeping Alive:

The Gateway will send the following to all connected Patrons at 10 second intervals:
- /messenger/gateway/who/active = <string: gateway OSC UID>

The Patrons will respond to this with:
- /messenger/gateway/me/patron = <string: patron OSC UID>
If a Gateway or Patron does not receive these messages for 30 seconds, it shall assume that the other has gone away and disconnect.


##  Integrating Your App with a Gateway: Step 3 – Leaving the Gateway:

The patron must announce that it is leaving the gateway via the unicast channel:
- /messenger/gateway/me/leave = <string: patron OSC UID>

The gateway must announce that it is shutting down via the unicast channel:
- /messenger/gateway/me/shutdown = <string: gateway OSC UID>

