# What Is This?

It is a mechanism for boostrapping RFC 6455 Websockets over an HTTP/2
stream. Other non-websockets streams can continue to use the other
streams of the same connection.

# What Problem Is Being Solved?

Connection behavior is not part of the semantic HTTP contract that
remains available between versions of the protocol. 6455 relies on
HTTP/1.1 connection behaviors that are not available in HTTP/2.

As a practical matter this means that servers that wish to transition
to HTTP/2 and also offer WebSockets need to run two servers on two
ports. This arrangement has the following problems:

* Administrative burden of running two servers on two ports

* Both servers cannot use the old {name, port} tuple, so any existing
  deployed markup will need to change for one of them. This may not
  even be possible.

* This arrangement creates two congestion control contexts which would
  operate better as one.

* The situation dis-incentives the transition to HTTP/2

# Why do we still need RFC 6455 - isn't HTTP/2 already bidirectional?

Chosing a shim on top of 6455 instead of doing a full native
integration into HTTP/2 is indeed a design choice. It is informed
largely by

* Previous efforts in this space to boil the ocean have shown there is
  insufficient interest to bring a solution to market. The marginal
  wins do not justify the complexity.

* Belief that the core problems with the current situation are driven
  by the lack of a unified serving architecture and can be satisfied
  at low risk (with low complexity) using this scheme. The other gains
  a full integration would provide, such as prioritization and
  multiplexing between websockets messages, are only considered minor
  nice-to-haves by the community.

# Instead of overloading CONNECT, why not a new TUNNEL method?

Methods are generally end to end and, more importantly, HTTP version
independent. CONNECT is already a special snowflake in this
regard. Note that the only method 7540 defines is CONNECT - because
all the others are inherited from the semantic layer of
723x. Extending it is fairly natural as its defined specifically for
HTTP/2 while a new method would not be well known as a version
specific mechanism.

# If not a new method, what about a new Frame Type? That's definitely h2 specific

HTTP/2 has a well defined stream state machine that this proposal
wants to reuse and introducing a new frame type would add a
significant amount of complexity to the mechanism in order to extend
those states. CONNECT is already special - this confines the
specialness to that case only.


<!--
kramdown-rfc2629 draft.mkd | tee draft.xml ; xml2rfc draft.xml
-->
