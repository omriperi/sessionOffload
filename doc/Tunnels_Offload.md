# IP Tunnel Offload

-------- DRAFT -----------

With openOffload it will be possible to offload also IP Tunnels into the underlying hardware.

A service called `ipTunnelTable` will be introduced, for CRUD operations on the offloaded sessions.

Through this service it will be possible to offload several kinds of IP Tunnels, where the common configuation between them is the `match` criteria, which indicates which IP will be used for tunnel creation / termination.

### Matching criteria

Since IP tunnels are L3 tunnels, the only matching criteria will be source subnet & destination subnets.



### Having one service for all the IP Tunnels

Pros:

- Possibility to get all sessions at once (to see the route every IP packet is going through)
- Adding more IP tunnel types will require less work from the service side

Cons:

- IP Tunnels can be completely different, forcing us to share configuration on the ip tunnels
- Usage of `tunnelId` is needed. This parameter can be "extra" since some of the tunnels will have their own identifier other than `tunnelId` (e.g. SPI for IPSec)



## Tunnel Chaining

This section will provide information regards tunenl chaining, or "IP in IP". Where packet should be encapsulated / decapsulated from several tunnels.

### Outgoing tunnel chaining

Problem with having several tunnels with the same mathcing criteria is we don't know which one we should apply on the packet, in a case where, for example, we're having two tunnels created on the same match - which one is going to be applied on the outgoing packet?

#### Option 1 - Not enabling two tunnels with the same match 

Not enabling the same match, and doing every encapsulation one after another

#### Option 2 - Providing tunnel with an action

Having multiple tunnels with actions attached to them (e.g. OpenFlow)

i.e. On the offloading of GRE Tunnel it'll be decided that the next action will be IPSec

#### Option 3 - Have a predefined order of actions

We'll have a pre-defined tunnel order, and this way it will be 
determined how the pipe is going to be implemented


### Outgoing tunnel chaining

Will be defined according to the solution of the outgoing tunnels