# Droid-Assissted-Pick-up
Droid-Assissted Pick-up and Drop-Off MILP Optimisation problem
Designing a Droid–Assisted Pickup and Delivery System on a Time–Space Network

Autonomous delivery droids are increasingly deployed to support urban logistics operations. Droids are capable of performing local pickup and delivery tasks but possess only
limited operating range; once their available energy is depleted, they must be collected
by a truck and transported before they can be used again. Trucks move freely across the
service region, carry parcels as well as multiple droids, and redeploy droids at suitable
locations. The objective of this assignment is to design a service network that coordinates
truck activities and droid excursions so that all known pickup and delivery requirements
are satisfied.

Spatial and temporal structure. The service region is partitioned into a set of hexagonal zones of equal area, denoted by Z. Each zone has up to six neighbors; let N(z) ⊆ Z
denote the set of zones adjacent to z ∈ Z. The depot is located at the centroid of a
designated depot zone z0 ∈ Z. Travel time from the centroid of a zone to the centroid
of any adjacent zone is one time interval. Time is discretized into equal-length periods
indexed by t ∈ {1, . . . , T}, where T denotes the final period of the planning horizon. We
interpret period t as the time interval [t, t+1), so a point (z, t) represents being in zone
z during period t. Vehicles start at the beginning of period 1 and must be back at the
beginning of period T. In each period, a vehicle either remains in its current zone or
travels to an adjacent zone, and its location is updated for the next period accordingly.
Either vehicle type may remain idle for any number of periods, consuming time without
expending distance or energy.

Vehicle movement and service rules. A truck requires one period to travel to an
adjacent zone and is not subject to any energy constraints. To perform a pickup or
delivery in a zone, a truck must remain stationary in that zone for one full period, during
which the loading or unloading operation is completed. A truck possesses two independent
capacities: one limiting the number of parcels it may carry and one limiting the number
of droids it may transport.

A droid likewise requires one period to move to an adjacent zone. Each droid has a
parcel capacity and an energy state. While operating independently, a droid expends one
unit of energy for every movement it performs and for every pickup or delivery action
it performs; waiting consumes no energy. A droid may perform pickup and/or delivery
actions while located in its current zone during period t, subject to a per-period service
cap: during any period t, a droid may serve at most Smax requests (pickups and/or
deliveries in total) in its current zone.

Transfers and service actions may occur during the same period when vehicles are
co-located in the same zone. For example, if two vehicles are together in zone z during
period t, a parcel may be transferred between them during period t, and the receiving
vehicle may complete a service in zone z during that same period.
Each droid begins the planning horizon fully charged. While operating independently,
a droid cannot replenish energy on its own. A droid may be collected by a truck only
when both vehicles are in the same zone during the same period; once collected, it may
be transported and redeployed later in the horizon. When a droid is carried by a truck,
it does not consume energy and its location coincides with the carrying truck’s location
for the periods in which it remains onboard.

Excursions. For the purposes of this assignment, the interval between the moment a droid
departs from a truck (i.e., leaves a truck to operate independently) and the moment it
next rejoins any truck (i.e., boards a truck) is termed an excursion. Range feasibility will
be expressed in terms of limits on the number of energy-consuming actions a droid may
execute during such excursions; in the scenarios below, this will be specialized first to a
no-recharging setting and later extended to allow recharging.
Pickup and delivery requests. The system must fulfill a known set of service requests.
A pickup request is defined by a zone and a release time; service may occur at any period
at or after its release time, and the collected parcel must eventually be transported to the
depot. A delivery request is defined by a zone and a due time; service may occur at any
period up to and including its due time, and every delivery originates from the depot.
Unless otherwise stated in the dataset, each delivery parcel is available at the depot at
the start of the horizon. Either a truck or a droid may perform a request, provided it is
in the required zone at an admissible service time and the relevant capacity constraints
are respected.

Parcels carried by a droid remain with that droid until they are delivered, returned to
the depot, or transferred to another vehicle. Parcels carried by a truck remain on board
until delivered, returned to the depot, or transferred. The scenarios below specify which
request types and which transfer options are active in each case.
Capacities, loading, and transfers. Each request corresponds to exactly one parcel
and consumes one unit of parcel capacity while it is carried. Trucks have a parcel capacity
and a droid-carrying capacity; droids have a parcel capacity. Truck parcel capacity applies
to all parcels onboard the truck, regardless of origin or which vehicle will ultimately serve
them. A droid may board or leave a truck only when both are in the same zone during
the same period, and truck droid capacity must never be violated.
Parcel transfers are permitted only when the involved vehicles are co-located in the
same zone during the same period. All transfers must respect the resulting parcel capacities of the involved vehicles. Delivery parcels are available at the depot in period 1
and may be loaded and transferred at the depot in period 1 without consuming a time
period.

In all settings, the objective is to minimize total transportation cost, including a fixed
cost for deploying a truck. The variable cost for a droid is proportional to its energy
consumption (one unit per movement and one unit per service action). The variable cost
for a truck is based on its traversed travel and service activity; whether staying/waiting
in a period incurs cost when no service is performed is specified by the arc-cost data
provided with the dataset.
The subsequent tasks in this assignment ask you to construct optimization models
that enforce these requirements under progressively richer operational assumptions.
Assignment Tasks
For each scenario, formulate a complete optimization model on the given time–space network. Your formulation must define all decision variables, constraints, and the objective
function in a way that is directly implementable on the provided dataset. Implement
each model and report the requested outputs.
1. Scenario 1: Baseline Model – Truck–Droid Deliveries with Fixed Droid
Range.

In this scenario, only delivery requests are present. Trucks and droids move on the
shared time–space network and may wait. Parcels originate at the depot at the
start of the horizon and may be carried by trucks and/or droids; truck–droid parcel
transfers are allowed only when the two vehicles are co-located in the same zone
during the same period. Droids do not recharge: each droid has a fixed total energy
budget R over the full horizon, where one unit of energy is consumed for each ground
movement and for each delivery action performed by the droid; waiting and riding
on a truck consume no energy. During any period t, a droid may serve at most
Smax requests in its current zone. All trucks and droids start at the depot and must
return to the depot by period T, and no parcels may remain on any vehicle at the
end of the horizon.

Report. Provide (i) the number of trucks deployed and the resulting truck and
droid time–space trajectories, (ii) which vehicle ultimately performs each delivery
request (and when), and (iii) a figure illustrating representative truck routes and
droid excursions.

3. Scenario 2: Deliveries with Droid Recharging and Multiple Excursions.
Scenario 2 uses the same delivery requests, fleets, transfer rules, capacities, and
service rules as Scenario 1. The only change is the droid range semantics: whenever
a droid rides on a truck for any period, it is considered fully recharged; thus, each
time the droid next operates independently (i.e., after leaving a truck), its available
energy is reset to the full range R. Equivalently, every excursion (from leaving a
truck to the next time it boards a truck) has its own budget of R energy-consuming
actions. All other requirements, including the per-period droid service cap Smax and
the end-of-horizon emptiness condition, remain in effect.
Report. Present the revised formulation with a clear description of how the Scenario 1
energy constraint is replaced by excursion-based constraints. Report (i) truck and
droid trajectories, (ii) the number and timing of excursions per droid, and (iii) a
figure illustrating at least one droid executing multiple excursions and indicating
where range resets occur.

5. Scenario 3: Pickup and Delivery with Truck–Droid and Droid–Droid
Transfers.
Scenario 3 extends Scenario 2 by adding pickup requests and allowing droid–droid
parcel transfers in addition to truck–droid transfers. Pickup requests have release
times and generate a parcel when served; each pickup parcel must be transported to
the depot by period T. Deliveries remain as before (each delivery parcel originates
at the depot and must be served by its due time). Parcels may be carried by
trucks and droids subject to capacities, and may be transferred between any two colocated vehicles when such transfers are enabled (truck–droid and droid–droid). The
excursion-based recharging behavior from Scenario 2 remains unchanged, and the
droid per-period service cap Smax applies to the total number of pickup and delivery
actions performed by that droid in that period. As in the previous scenarios, all
vehicles must return to the depot by T, and no parcels may remain on any vehicle
at the end of the horizon.

Report. Provide the full formulation including explicit parcel movement and transfer
constraints. Report (i) the integrated plan of trucks and droids for both pickups and
deliveries, and (ii) illustrations highlighting truck–droid and droid–droid transfer
points in the time–space network.
