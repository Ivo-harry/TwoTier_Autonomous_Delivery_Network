# Optimizing a Two-Tier Autonomous Delivery Network

This repository contains a Mixed-Integer Linear Programming (MILP) model designed to optimize an urban delivery network using a combination of carrier vans and autonomous ground rovers. 

The core challenge addresses range anxiety in last-mile autonomous delivery. While rovers are efficient for local drop-offs, they have strict battery limits. Vans, which have no range restrictions, act as mobile micro-hubs, transporting packages, carrying multiple rovers, and deploying/picking them at optimal locations. The model uses a time-space network to synchronize these vehicle routes, package handoffs, and battery constraints.

## Spatial and Temporal Architecture
The optimization framework discretizes both geography and time to track the precise state of every asset.
* **Geography:** The service region is partitioned into an interconnected grid of discrete zones. A central hub serves as the origin and terminus for the fleet.
* **Time:** The operational day is divided into uniform time steps. Moving from one zone to an adjacent zone consumes exactly one time period. The solver guarantees that all active vehicles return to the central hub by the final time period, completely empty of cargo.

## Fleet Mechanics and Capacity Constraints
The model controls two distinct vehicle types, each governed by independent operational rules.
* **Vans:** Operate without energy limitations and require one full time step to move between zones or remain stationary to execute loading operations. Vans possess two independent capacity limits: a maximum package capacity and a maximum rover-carrying capacity.
* **Rovers:** Operate autonomously on the ground but are restricted by a finite battery limit. Operating independently consumes one unit of energy for every zone transition and one unit for every service action (dropping off or collecting a package). Rovers also have a localized throughput limit, capping the maximum number of service actions they can execute within a single time step.

## Progressive Optimization Phases
The model is formulated in three progressive phases to handle escalating operational complexities.

### Phase 1: Baseline Drop-off Network
Focuses strictly on outbound logistics. All packages originate at the central hub and possess specific deadline constraints. Vans transport rovers into the field, and packages can be handed off from a van to a rover provided they are co-located in the same zone during the exact same time period. Rovers have a strict, non-renewable energy budget for the entire planning horizon and cannot recharge.

### Phase 2: Excursion-Based Recharging
Introduces dynamic energy replenishment. The rigid lifetime battery limit is replaced by an excursion-based constraint. Whenever a depleted rover docks back into a van and rides as cargo, it is considered fully recharged. This allows the solver to formulate multi-sortie routes, where a single rover can be deployed, retrieved, recharged, and redeployed multiple times across different sectors of the city.

### Phase 3: Integrated Collections and Relay Handoffs
Upgrades the system to a full Pickup and Delivery Problem (PDP). It introduces collection tasks bounded by strict release times, requiring rovers or vans to pick up field-generated packages and transport them back to the hub. This phase unlocks maximum operational flexibility by enabling direct rover-to-rover package transfers in the field (in addition to van-to-rover transfers), allowing for advanced, multi-leg relay routing.

## Objective Function
The solver seeks to satisfy all delivery deadlines and collection time windows while minimizing the total transportation cost. The objective function minimizes:
1. A fixed cost for deploying each van from the hub.
2. A variable cost applied to van movement (and conditionally to van idling, depending on the network graph).
3. A variable cost tied directly to the total energy consumed by the rover fleet during ground operations.

## Technologies Used
* **Python** for core logic and data structures.
* **Gurobi** for mixed-integer linear programming and exact optimization.
* **NetworkX** for time-space multigraph generation and spatial routing constraints.
* **Matplotlib** for spatial-temporal visualizations and Gantt chart scheduling.

## How to Run

1. Clone this repository:
   ```bash
   git clone [https://github.com/yourusername/two-tier-delivery-network.git](https://github.com/yourusername/two-tier-delivery-network.git)
