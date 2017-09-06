[//]: # (Image References)
[image1]: ../update_equations.png

---
## Model
---

The implementation consists of a kinematic model to control the vehicle around the track.
The model optimizes actuator inputs based on a cross track error (CTE) calculation of a given set of reference waypoints and a best fitted polynomial of the cars trajectory. The calculated trajectory resulted from a given set of actuator inputs that yields the lowest cost in the CTE function is used to control the vehicle.

**State Vector:**

* x: cars x global position
* y: cars y global position
* psi: vehicle angle in radians
* ν: vehicle velocity
* delta: steering angle in radians
* a: throttle/brake (+1 for throttle, -1 for brake)

**Actuators:**

* delta: steering angle in radians
* a: throttle/brake (+1 for throttle, -1 for brake)

**Update Equations:**

![Update Equations for MPC][image1]

---
## Timestep Length and Elapsed Duration (N & dt)
---

**N and dt:**

* N is the number of timesteps on the horizon
* dt is how much time elapses between actuations

Final N (10) and dt (0.1) values were chosen through experimentation. I found that large N values caused the solver to work unnecessarily hard and cause erratic behavior in vehicle actuations. A small N value seemed to just not have enough future information to calculate the optimal trajectory that best matched the reference waypoints, this also caused erratic actuation control. The final dt value was chosen because it kept calculations to a minimum while helping model a 100ms latency. Larger dt values then .1 cause the model to be to slow to respond correctly on the track.

---
## Latency
---

Latency compensation was accomplished by choosing a dt value of 0.1 (100ms) and using the returned solver values one timestep into the future. (Lines 253-254 `mpc.cpp`)
