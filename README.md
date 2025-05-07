# Efficient-Deployment-and-Operation-of-Healthcare-Units
Determines the optimal placement of the M healthcare units and assigns communities to receive service so that the maximum population-weighted distance minimized. Proposes an efficient method that minimizes the total travel distance required to distribute medical equipment.

The problem description provided by Sabancı University can be found as the optchall_25.pdf file. 

## Decision Variables

- **`x_{ij}`**: Binary variable, 1 if community *i* is assigned to healthcare unit *j*, 0 otherwise.  
- **`y_j`**: Binary variable, 1 if a healthcare unit is deployed at location *j*, 0 otherwise.  
- **`D_{max}`**: The maximum population-weighted distance across all communities (our objective).

## Parameters

- **`N`**: Number of communities  
- **`M`**: Number of healthcare units to deploy  
- **`C`**: Maximum capacity of each healthcare unit  
- **`P_i`**: Population of community *i*  
- **`d_{ij}`**: Euclidean distance between community *i* and candidate site *j*  

## Objective

Minimize the maximum population-weighted distance:
  
![Objective](https://latex.codecogs.com/svg.image?\min%20D_{max})

## Constraints

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
