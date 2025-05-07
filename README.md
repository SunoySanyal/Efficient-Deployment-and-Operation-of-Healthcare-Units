# Efficient-Deployment-and-Operation-of-Healthcare-Units
Determines the optimal placement of the M healthcare units and assigns communities to receive service so that the maximum population-weighted distance minimized. Proposes an efficient method that minimizes the total travel distance required to distribute medical equipment.

The problem description provided by Sabancı University can be found as the optchall_25.pdf file. 

## Table of Contents

- [Healthcare Unit Deployment Formulation (MinMaxLocation)](#healthcare-unit-deployment-formulation)
- [Capacitated Ambulance Routing Formulation (CVRP)](#ambulance-routing-formulation)  

---
## Healthcare Unit Deployment Formulation

### Decision Variables

- **`x_{ij}`**: Binary variable, 1 if community *i* is assigned to healthcare unit *j*, 0 otherwise.  
- **`y_j`**: Binary variable, 1 if a healthcare unit is deployed at location *j*, 0 otherwise.  
- **`D_{max}`**: The maximum population-weighted distance across all communities (our objective).

### Parameters

- **`N`**: Number of communities  
- **`M`**: Number of healthcare units to deploy  
- **`C`**: Maximum capacity of each healthcare unit  
- **`P_i`**: Population of community *i*  
- **`d_{ij}`**: Euclidean distance between community *i* and candidate site *j*  

### Objective

Minimize the maximum population-weighted distance:
  
![Objective](https://latex.codecogs.com/svg.image?\min%20D_{max})

### Constraints

1. **Assignment**  
   Each community must be assigned to exactly one healthcare unit:  
   ![Assignment](https://latex.codecogs.com/svg.image?\sum_{j=1}^N%20x_{ij}%20%3D%201%2C%20\forall%20i\in\{1,\dots,N\})

2. **Deployment condition**  
   You can’t assign a community to a unit unless it’s deployed:  
   ![DeployCond](https://latex.codecogs.com/svg.image?x_{ij}\le%20y_j%2C%20\forall%20i,j\in\{1,\dots,N\})

3. **Unit count**  
   Exactly *M* healthcare units must be opened:  
   ![UnitCount](https://latex.codecogs.com/svg.image?\sum_{j=1}^N%20y_j%20%3D%20M)

4. **Capacity**  
   The total population assigned to unit *j* cannot exceed its capacity:  
   ![Capacity](https://latex.codecogs.com/svg.image?\sum_{i=1}^N%20P_i%20x_{ij}\le%20C\,y_j%2C%20\forall%20j)

5. **Weighted distance definition**  
   Define each community’s weighted distance:  
   ![DistDef](https://latex.codecogs.com/svg.image?d_i\ge%20x_{ij}\,P_i\,d_{ij}\,,\;\forall\;i,j)

6. **Link to objective**  
   Enforce that *Dₘₐₓ* is the maximum of all *d_i*:  
   ![Link](https://latex.codecogs.com/svg.image?d_i\le%20D_{max}\,,\;\forall\;i)


## Ambulance Routing Formulation

### Decision Variables

- **`z_{ij}`** — Binary, 1 if an ambulance travels from unit *i* to unit *j*.  
- **`u_j`** — Continuous, load (equipment demand) delivered to unit *j*.  

### Parameters

- **`H`** — Set of deployed healthcare units (from Stage 1).  
- **`Q = 10{,}000`** — Capacity of each ambulance.  
- **`D_{ij}`** — Euclidean distance between units *i* and *j*.  
- **`R_j`** — Equipment demand at unit *j*.  

### Objective

Minimize total distance traveled by all ambulances:

![Obj](https://latex.codecogs.com/svg.image?\min%20\sum_{i\in%20H\cup\{0\}}\sum_{j\in%20H}%20D_{ij}\,z_{ij})

### Constraints

1. **Visit exactly once**  
   Each unit *j* must be visited by exactly one incoming route:  
   ![C1](https://latex.codecogs.com/svg.image?\sum_{i\in%20H\cup\{0\}}%20z_{ij}%20=%201\,,%20\forall\,j\in%20H)

2. **Ambulance count**  
   At most *K* ambulances depart the depot:  
   ![C2](https://latex.codecogs.com/svg.image?\sum_{j\in%20H}%20z_{0j}%20\le%20K)

3. **Flow conservation**  
   For each unit *j*, arrivals equal departures:  
   ![C3](https://latex.codecogs.com/svg.image?\sum_{i\in%20H\cup\{0\}}%20z_{ij}%20=%20\sum_{k\in%20H}%20z_{jk}\,,%20\forall\,j\in%20H)

4. **Capacity meets demand**  
   Equipment load constraints:  
   ![C4a](https://latex.codecogs.com/svg.image?u_j%20\ge%20R_j\,,%20\forall\,j\in%20H)  
   ![C4b](https://latex.codecogs.com/svg.image?u_i%20-%20u_j%20+%20Q\,z_{ij}%20\le%20Q\,,%20\forall\,i,j\in%20H)

5. **Subtour elimination**  
   Prevents disconnected loops:  
   ![C5](https://latex.codecogs.com/svg.image?u_i%20-%20u_j%20+%20Q\,z_{ij}%20\le%20Q%20-%20R_j\,,%20\forall\,i,j\in%20H)
