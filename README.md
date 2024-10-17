# Heat Transfer Analysis in a Freezer System

This repository contains the analysis and modeling of a heat transfer problem in a freezer system. The scenario involves cooling bottles of wine using a small freezer, accounting for various environmental and thermal properties.

## Problem Overview
The system includes:
- A freezer with an evaporator cooling capacity of 100 W.
- A blend of two different wine varieties (Petit Verdot and Chardonnay) with known thermal properties.
- A freezer environment where the external temperature varies from 31°C to 20°C over time.

The goal is to analyze how long it will take to cool the wine to the desired temperature of 2°C, considering the cooling capacity, wine properties, and freezer characteristics.

## Key Data

### Wine Properties

| Variety      | Percentage | Density (kg/m³) | Specific Capacity (J/kg·K) | Thermal Conductivity (W/m·K) |
|--------------|------------|-----------------|----------------------------|-----------------------------|
| Petit Verdot | 65%        | 1070            | 4042                       | 1.194                       |
| Chardonnay   | 35%        | 1025            | 4015                       | 1.255                       |

### Freezer and Environmental Parameters

- **Freezer Capacity**: 100 W
- **Initial Wine Temperature**: 20°C
- **Freezer Temperature**: 2°C
- **External Temperature**: 31°C (noon), 33°C (3 PM), 20°C (midnight)
- **Freezer Dimensions**: As a cylindrical shape with the following dimensions:
  - Bottle height: 40 cm
  - Diameter: 20 cm

## Mesh Sensitivity

The mesh sensitivity analysis concluded that an element size of 7.5 cm suffices, meaning that decreasing the mesh element size further has little impact on the accuracy of the simulation.

## Setup

Both boundary conditions and material properties were defined as follows:

### Wine Material Properties
The wine material properties were defined as:

$$
0.65X_{Petit Verdot} + 0.35X_{Chardonnay}
$$

Where X represents material properties such as density, specific heat capacity, and thermal conductivity. Wine was modeled as a solid material.

### Freezer Domain
The freezer was modeled as air at 25°C with a reference pressure of 1 atm. A buoyancy model with a reference temperature of 25°C was chosen, with a gravitational force of -9.81 m/s². A k-epsilon turbulence model was selected with medium intensity.

### Freezer Boundary Conditions
The freezer boundary conditions were of the type Wall. All side boundaries and the top surface boundary were modeled as no-slip walls with a flux type heat transfer. The evaporator wall was modified to incorporate the cooling capacity and chosen to be the smaller 50 cm x 110 cm side. The bottom was modeled as adiabatic heat transfer. The boundary with the wine bottles was modeled as an interface boundary type with a conservative heat flux heat transfer.

### Wine Bottle Domain
The wine bottle domain was modeled as a solid, with the material properties defined in the wine section above.

### Interface
The interface between the wine bottles and the freezer domain was a fluid-solid interface with conservative heat flux heat transfer.

## Mesh Visualization

Below are the vertical and horizontal section planes of the freezer with the wine bottles, illustrating the mesh structure.

![image](https://github.com/user-attachments/assets/ec0e7024-cd5a-4172-adef-201b67193d24)


_Figure 3: Vertical section plane of the freezer with wine bottles_

![image](https://github.com/user-attachments/assets/0dfea200-8754-491c-83bb-3ca66aebb74d)

_Figure 4: Horizontal section plane of the freezer with wine bottles_

## Objective
To determine the cooling time of the wine under these conditions, and how external factors affect the system's efficiency.

---
