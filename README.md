# TSPPD Test Instance Library

This repository contains problem files used to test models for solving the Traveling Salesman with Pickup and Delivery. Problem types are divided by subdirectory. `random-uniform/` contains problem instances with random pickup and delivery locations distributed uniformly over 1000x1000 squares. `grubhub/` contains problem instances generated from observed meal delivery requests at Grubhub and actual travel `grubhub/png/` contains graphical representations of test instances, along with optimal tours.

The `.tsp` files use an extension of the TSPLIB format which adds precedence relations in a `PRECEDENCE_SECTION`. For instance, the following TSPPD instance requires `+i` precede `-i` in any feasible tour, for `i` in `{0, 1, ..., n}`.

```
NAME: random-2-232
TYPE: TSP
COMMENT: size=2 seed=232
DIMENSION: 6
EDGE_WEIGHT_TYPE: EUC_2D
NODE_COORD_SECTION
+0 224 358
-0 224 358
+1 179 754
-1 738 117
+2 557 736
-2 144 640
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
    "name": "random-2-232",
    "comment": "size=2 seed=232",
    "nodes": ["+0", "-0", "+1", "-1", "+2", "-2"],
    "precedence": {
        "+0": "-0",
        "+1": "-1",
        "+2": "-2"
    },
    "edges": [
        [    0,    0,  399,  568,  504,  294],
        [    0,    0,  399,  568,  504,  294],
        [  399,  399,    0,  848,  379,  120],
        [  568,  568,  848,    0,  645,  792],
        [  504,  504,  379,  645,    0,  425],
        [  294,  294,  120,  792,  425,    0]
    ]
}
```
