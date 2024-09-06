# tsptw_essenceprime
Travelling Salesman Problem with Time Windows model in essence prime

## Requirements

[SavileRow](https://www-users.york.ac.uk/peter.nightingale/savilerow/)

## Problem definition

As defined by [Ferreira (2010)](https://doi.org/10.1016/j.disopt.2010.04.002), 
an agent must find an order to service each node in a graph, starting and 
finishing at a "depo" node, minimising the duration taken to service each node,
where each node must be serviced within a *time window*. Should the agent arrive
at a node early, it must wait for the start of the time window. The agent cannot
be servicing a node after the time window expires.

## Usage instructions

To run the example graph, use:

```bash
savilerow tsptw.eprime test_graph.param -chuffed -run-solver -solutions-to-stdout
```

## Param file explination

The graph is defined as three matrices, `graph`, `window` and `service_time`.

`graph` defines the time taken to move between nodes.
An entry in this matrix `[x, y]`, determines the time taken to move from node 
*x* to node *y*. A value in this matrix of `-1` represents no path between these
nodes.

`window` defines the times each node can be serviced. `window[x]` will provide 
the acceptable time window tuple for node x. `window[x, 1]` gives the start of
the window, `window[x, 2]` gives the end of the window. These values must be 
larger than or equal to 0. Servicing can start at the start of the window, and
the must finish at the latest at the end of the window.

`service_time` defines the time each node requires to be serviced.

`max_duration` is the upper limit of duration the solver will search for.  
This can be pre-calculated fairly easily:  
  max((depo deadline + depo service time), max(non-depo deadline + non-depo service time + time to depo))

## Result explination

`arrival_time` - the time the agent arrives at each node  
`duration` - the agent's active time travelling, waiting, or servicing nodes  
`order` - the order the agent visits intermediate nodes. The agent starts and ends at node 1, which is not shown.  
`service_depo_first` - whether the agent services the depo first or last  
`waiting_time` - the time the agent waits at each node  