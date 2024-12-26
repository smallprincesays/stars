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

