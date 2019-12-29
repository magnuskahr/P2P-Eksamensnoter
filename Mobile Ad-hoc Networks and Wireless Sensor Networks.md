# 5. Mobile Ad-hoc Networks and Wireless Sensor Networks

{{TOC}}

~~Using *link state* each peer periodically sender deres naboer, deres pris for linket, deres ID og sådan, og så ved man altid hvordan man router til den billigste. Det er dyrt at gøre sådan, kræver en del trafik~~

~~Der er også noget der hedder en distance vector, tror det er noget med at noder deler deres priser for at route til andre steder; og så kan en node via andre noder finde den billigste ting.~~

## Motivation
## Ad-Hoc networking

Ad hoc networking is the idea of enabling radio connected devices to collaborate with each other without having to rely on a centralised station.

Doing such thing, encounters a number of challenges.

> * **Problems**
> * `limited devices` these devices are often limited in range, bandwith, cpu, storage or battery life - making it necessary to take such limitations into account
> * `portable` well guess what, portable devices are .. portable! They move around! And that is also important to think about

Also being that it typically is radio waves which is used, that itself brings some problems, where signal strength can be unpredictable, radio waves can be blocked or absorbed, radio communication can be unidirectional where a can hear b, but b cant hear a, and broadcasting in itself can be a security risk.

It is clear that working with these technologies brings a lot of problems with it, but what they enable can be used for remote areas, unplanned meetings, to extend existing infrastructure, emergency situations, military, sensor networks and so on.

## MANET routing
_Mobile Ad-hoc Networking_

### Goals

* we need minimal control overhead
	* really important, we are dealing with weak devices, finite power resource 
* minimal processing overhead
	* Since networks are graphs, which are heavy lifting (calculation)
	* we cant do heavy computation on these devices
	* therefor are routings must be “simple”
* multi-hop routing capability
	* this is just necessary, if we cant see E, but B can, we can route via B
* dynamic topology maintenance
	* protocols must deal with this, because its a fact
* loop prevention
	* please do not loop, its a waste of energy

### Pro vs Reactive

There are to ways to route in a MANET, where the routing either can be proactive or reactive.

> * **Proactive**
> `Routing table` In a proactive routing environment, each node knows the network which is laid out in a routing table
> `Route changes` therefor, some bandwidth and energy will also be used to maintain route tables when they change, like if a node is moved
> `Low latency` but that it does know the difference routes, makes the proactive approach have low latency
> `Control overhead` but it is at a cost of high control overhead by maintaining and keep updating the routing table


> * **Reactive**
> * `(almost) No state` each node contains no state, or close to, meaning that they only keep what is necessary
> * `route on demand` therefor, routes are discovered on demand, when a message is needed to be sent, but is remembered though. If a remmebered route is broken, it needs to discovery a new again on demand.
> * `routing = costly` when suddenly a routing is needed, it is more costly though, because energy is needed for discovery. 

### Protocols

Great to lets look at a few different protocols!

#### DSDV

Destination Sequence Distance Vector. Is a great name, am I right? It is the first protocol we are gonna look at though, and it is proactive!

> * **DSDV**
> * Proactive
> * `Routing table` being that it is proactive, it of course has a routing table, where it keeps track of:
> * `* Destination`
> * `* Next Hop`
> * `* Cost`
> * `* sequence number` and a sequence number. Whenever a new route is received, it will be stored in the routing table if it has a higher sequence number; if it equal though, it will keep the one with the lowest cost.
> * `damped updates` When ever a node moves, it waits a bit before telling about it, until it is settled; so we dont get updates of a ton of routes which will replace each other. A node will infrequently send a full update, but whenever a change is found in the routes, if it can send that information as a piggy bag ride in a single network package - it does so.
> * `loop prevention` To combat loops, the sequence number is an important aspect of the protocol. Each node will keep track of its own number, and when ever it send out an update of its route, it bumps the number by two. If a nabo node finds that the route suddenly is broken, it will send out an update with a cost of $\infty$ and a sequence number bumped by one.

#### AODV

Adhoc Ondemand Distance Vector is another protocol, which is reactive this time.

> * **AODV**
> * Reactive
> * `no routing table` and there has no routing table!
> * `route request` when a route is needed, a route request is broadcasted into the network, which each other node will propagate 
> * `route response` when the request comes to the destination or a node that knows it, a _route response_ is send back using the reverse path. It does helped by every node which remembers a broadcastId from the request, which now will begin to remember to path.
> * `destination sequence number` such a response contains a sequence number, so if multiple respones comes back, the peer will use the route with the highest number

In AODV link failures are handled by also marking it at a $\infty$ cost, propagated to a peers nabos which uses the route. Now whenever a route to the node is needed again, it will be discovered using a higher sequence number.

* properties
	* cheaper only maintained active routes
	* small, simple table lookups, comparing numbers etc
	* functions well in low traffic
	* in high traffic, the initial cost can be high


#### DSR

And just for fun, lets get a quick overview of yet another reactive protocol, called Dynamic Source Routing.

> * **DSR**
> * Reactive
> * `request and replies` it too uses request and replies, but in a very different way, where the intermediate nodes wont remember anything from it
> * `Request builds path` when the request is broadcaster, it is modified to keep the path in itself. Meaning that it start by containing A, then AB, then ABC and such
> * `replies` if the network is bidirectional, the reply can simply go through the reverse path, but is it unidirectional, the destination can simple send a request against the source, and in that message let the request-path piggyback to prevent loop.
> * `radio geared` Here it is shown that it truly is a radio-geared protocol because it supports being unidirectional. Also it can con
* ~~if memory is available, it can store the routes now if it has multiple to the same, if one breaks, it has a backup~~


### Energy Efficient
## Wireless Sensor Networks