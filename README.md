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

![image](https://github.com/user-attachments/assets/f7176a57-2294-4cfc-b621-415f1064a49e)

## Assumptions

- The freezer walls have a thickness of 6.5 cm and a thermal conductivity of 0.025 W/m·K.
- The evaporator covers the entire back wall of the freezer.
- The bottom of the freezer is perfectly insulated.
- Air inside the freezer is modeled using a k-epsilon turbulence model at 25°C.
- Heat transfer through the walls can be approximated using the equation:

  $$
  Q_{wall} = \frac{k_{wall}}{t_{wall}} (T_{env} - T)
  $$

## Model Setup
The model will simulate heat transfer from the external environment to the freezer, and from the wine bottles to the cooling system, factoring in environmental temperature fluctuations. The freezer walls' thermal resistance and the wine's thermal properties are also taken into account.

## Objective
To determine the cooling time of the wine under these conditions, and how external factors affect the system's efficiency.

---

