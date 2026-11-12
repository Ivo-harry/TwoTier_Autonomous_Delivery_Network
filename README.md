# Optimizing a Two-Tier Autonomous Delivery Network

This repository contains a Mixed-Integer Linear Programming (MILP) model designed to optimize an urban delivery network using a combination of carrier vans and autonomous ground rovers. 

The core challenge is that while rovers are efficient for last-mile drop-offs, they have strict battery limits. Vans, which have no range restrictions, act as mobile hubs—transporting packages, carrying multiple rovers, and deploying them at optimal locations. The model uses a time-space network to synchronize these vehicle routes, package handoffs, and battery constraints.

## Project Structure

The optimization model was built in three progressive phases to handle increasing operational complexity:

* **Phase 1 (Core Drop-offs):** A baseline model where vans deploy rovers to deliver packages. Rovers have a single, global battery allowance for the entire day and cannot recharge.
* **Phase 2 (Dynamic Recharging):** Introduces multiple "sorties." When a rover is retrieved and rides inside a van, its battery is restored, allowing it to be redeployed multiple times throughout the day.
* **Phase 3 (Integrated Collections & Handoffs):** The full model. It adds package collection tasks with specific time windows and allows packages to be handed off not just from van-to-rover, but directly from rover-to-rover in the field.

## Tech Stack

* **Language:** Python
* **Optimization:** [Insert Solver, e.g., Gurobi / CPLEX]
* **Libraries:** [Insert libraries, e.g., NetworkX, Pandas, Matplotlib for visualization]

## How to Run

1. Clone this repository:
   ```bash
   git clone [https://github.com/yourusername/two-tier-delivery-network.git](https://github.com/yourusername/two-tier-delivery-network.git)
