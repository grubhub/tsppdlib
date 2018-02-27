# TSPPD Test Instance Library

This repository contains problem files used to test models for solving the Traveling Salesman Delivery Problem with Pickup and Delivery. The sample instances have been provided by [Grubhub](https://www.grubhub.com) to advance research on the subject.

Problem types are divided by subdirectory:

* `instances/random-uniform/` contains problem instances with random pickup and delivery locations distributed uniformly over 1000x1000 squares.
* `instances/grubhub/` contains problem instances generated from observed meal delivery requests and actual travel time estimates.
* `instances/grubhub/json/` contains JSON versions of the Grubhub `.tsp` files with pre-computed edge costs.
* `instances/grubhub/png/` contains graphical representations of test instances, along with optimal tours.

The `.tsp` files use an extension of the TSPLIB format which adds precedence relations in a `PRECEDENCE_SECTION`. For instance, the following TSPPD instance requires `+i` precede -i in any feasible tour, for i in {0, 1, ..., n}.

The `.tsp` files use an extension of the TSPLIB format which adds precedence relations in a `PRECEDENCE_SECTION`. For instance, the following TSPPD instance requires `+i` precede `-i` in any feasible tour, for `i` in `{0, 1, ..., n}`.

```
NAME: grubhub-02-0
TYPE: TSP
COMMENT: size=2 instance=0
DIMENSION: 6
EDGE_WEIGHT_TYPE: EXPLICIT
EDGE_WEIGHT_FORMAT : LOWER_DIAG_ROW
EDGE_WEIGHT_SECTION
0
0 0
389 0 0
792 0 641 0
1357 0 1226 1443 0
961 0 1168 1490 741 0
NODE_COORD_SECTION
+0 968 397
-0 968 397
+1 696 258
-1 753 0
+2 34 964
-2 704 1000
PRECEDENCE_SECTION
+0 -0
+1 -1
+2 -2
EOF
```

TSPPD problems have start and end nodes, which are represented as `+0` and `-0` by  convention. `+0` typically is the start location and `-0` represents wherever the salesman ends up at the end of the tour. The travel cost from any node to `-0` may be coded as `0` to indicate that the final location of the route is the node that immediately precedes `-0`. A problem of size n pairs therefore has (n + 1) * 2 total nodes. The arcs connecting `-0` to other nodes should have zero cost if the end location of the route does not matter.

For convenience, `json` folders are also provided that contain data files with pre-computed arc costs. These may be easier to ingest in models than the `.tsp` format. For example, the above instance appears as the following in `json` format.

```json
{
    "name": "grubhub-02-0",
    "comment": "size=2 instance=0",
    "nodes": ["+0", "-0", "+1", "-1", "+2", "-2"],
    "precedence": {
        "+0": "-0",
        "+1": "-1",
        "+2": "-2"
    },
    "edges": [
        [    0,    0,  389,  792, 1357,  961],
        [    0,    0,    0,    0,    0,    0],
        [  389,    0,    0,  641, 1226, 1168],
        [  792,    0,  641,    0, 1443, 1490],
        [ 1357,    0, 1226, 1443,    0,  741],
        [  961,    0, 1168, 1490,  741,    0]
    ]
}
```

For each Grubhub instance, a `.png` file shows the courier location as a blue circle. Pickups are shown as green triangles with dashed lines to their associated deliveries, depicted as red squares.

![Grubhub test instance](instances/grubhub/png/grubhub-02-0.png?raw=true)

Optimal tours for these instances are shown connecting the courier to pickup and delivery locations. The directed path is shown as a solid line starting at the courier.

![Optimal tour](instances/grubhub/png/grubhub-02-0-optimal.png?raw=true)
