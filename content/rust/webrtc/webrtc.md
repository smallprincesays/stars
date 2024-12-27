+++
title = "On WebRTC"
date = 2024-12-26
+++

As part of a larger Rust project, I'm implementing WebRTC
to establish peer-to-peer connections in a mesh-like fashion.
My main goal is to add a state machine and replace some of 
the components that are currently slowing down the program.

To give context, WebRTC is an protocol and API for two peers
to negotiate a bi-directional secure connection and
an API to let developers use the protocol in their preferred
language. It additionally serves as an abstraction for many
internet protocols (e.g. SCTP, STUN/TURN, ICE, SDP) that would
have to be historically hand-rolled for peer connections. In this
post, I won't go into the nitty gritty of how WebRTC works under
the hood&mdash;fortunately, we can implement WebRTC without knowing
all the details 100% (I highly recommend reading through
[WebRTC for the Curious](https://webrtcforthecurious.com/) for
an amazing deep dive into these details).

Our goal is to establish a peer-to-peer connection between a
**client** and a **worker** with a *data channel*, which allows
to send arbitrary bytes across the network connection. To negotiate
the WebRTC connection between the client and worker, we need
a **signaling server**, which simply means a relayer that will
forward all client messages to the worker, and all worker messages
to the client.

![WebRTC System](../webrtc_system.svg)

Assume the client knows the ID for the worker it is attempting to connect to.
Defining the client as the "impolite peer" (i.e. the one that always
starts the negotiation), we can negotiate using the following steps (summarized version
of the WebRTC for the Curious book mentioned earlier):

1.  Client generates an offer. This offer consists of a local **Session
    Description Protocol** (SDP), which contains all the data formats
    (e.g. audio OPUS, video VP8) that this peer is willing to exchange.
    The client then sends this offer to the worker through the signaling
    server.

    At this point, the client has an <code class="il">RTCPeerConnection</code> object, with
    its local description set to the offer SDP.

2.  The worker receives the client's offer, and sets its <code class="il">RTCPeerConnection</code>
    object's remote description to the client's SDP. Additionally, it generates an answer, which
    is another SDP that may exclude some of the data formats that the offer SDP had. This ensures
    both peers are aware of the data formats they are willing to exchange.

    Similar to the client, the worker's <code class="il">RTCPeerConnection</code> both 
    local and remote descriptions correctly set.

3.  When the client receives the answer, it sets its remote description accordingly. At this point,
    both peers can send each other generated **ICE candidates**, which represent possible 
    addresses that this peer is available on. This process is necessary because either 
    peer could be behind private IP addresses in separate subnets, making traditional HTTP 
    connections impossible (routers would not know which host to send packets to).

    WebRTC instead uses a **network address translation** (NAT) mapping, which will map
    private IP addresses like <code class="il">192.168.0.1:7000</code>
    to public ones like <code class="il">5.0.0.1:7000</code> (essentially automated port forwarding).
    All the automation is packaged into the STUN/TURN/ICE protocols.

4.  As both the client and the worker receive ICE candidates from each other, they will use
    ICE ping packets to establish connectivity. Once the connection is established, we can 
    treat this as any normal socket and start sending data.

5.  In the background, the client and worker continue sending ICE candidates, and if a better
    Candidate Pair is found, the underlying connection is moved for the rest of the session
    under the hood.

Here is the current design for modeling this problem in a simple way:

1.  A client and worker attempt to establish a WebRTC connection.

2.  A **distributor** acts as a signaling server, with bidirectional **gRPC** stream
    connections with both the client and the worker. An important constraint here is that
    when the distributor is deployed on e.g. Google Cloud Run, there may be multiple
    instances of the distributor, so we require message passing between distributor
    instances to be a correct signaling server implementation.

With this constraint, the diagram turns into:

![WebRTC System with Multiple Distributor Instances](../multi_dist.svg)

For the initial system, we have been using Google **Pub/Sub** for inter-instance
communication. Here is an example of <code class="il">RtcMessage</code> protobuf
definitions for the worker (client definitions are very similar):

```proto
enum RtcEvent {
    OFFER = 0;
    ANSWER = 1;
    ACCEPT_ANSWER = 2;
    ICE_CANDIDATE = 3;
    LOCAL_ICE_CANDIDATE = 4;
    BLANK = 5;
    PING = 6;
}

message WorkerRtcMessage {
    int64 worker_machine_id = 1;
    RtcEvent event = 2;
    bytes message = 3;
}

service WorkerService {
    rpc NegotiatePeerConnection(stream WorkerRtcMessage) returns (stream WorkerRtcMessage);
}
```
