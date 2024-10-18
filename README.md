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

<img src="https://github.com/user-attachments/assets/ec0e7024-cd5a-4172-adef-201b67193d24" alt="Vertical section plane of the freezer with wine bottles" width="500"/>

_Figure 3: Vertical section plane of the freezer with wine bottles_

<img src="https://github.com/user-attachments/assets/0dfea200-8754-491c-83bb-3ca66aebb74d" alt="Horizontal section plane of the freezer with wine bottles" width="500"/>

_Figure 4: Horizontal section plane of the freezer with wine bottles_


## Objective
To determine the cooling time of the wine under these conditions, and how external factors affect the system's efficiency.

---






## Environmental Temperature Changes

A linear dependency was assumed during outside temperature changes. Temperature increased linearly from 12:00 to 15:00 and decreased linearly from 15:00 until midnight. This resulted in the following piecewise system of linear equations:

$$
\begin{aligned}
    &\frac{2}{3} \cdot (t + 31) & t \leq 3h \\
    &-\frac{13}{9} \cdot (t - 3) + 33 & \text{else}
\end{aligned}
$$

This expression was implemented in the simulation software as:

if(t < 3[hour], (2/3) * t + 31[C], (-13/9) * (t - 3) + 33[C])

The temperature equation was coupled with a heat flux boundary condition across the freezer walls, given by:

$$
\left(\frac{0.025}{0.065}\right) \cdot (T_{out} - T)
$$

Where \( T \) is the temperature inside the freezer, and \( T_{out} \) is the environmental/outside temperature.

### Graph of Outside Temperature vs Time

![image](https://github.com/user-attachments/assets/022269a9-f7a7-4801-8193-d6e24204c2e7)

_Figure 5: outside temperature vs time_






![image](https://github.com/user-attachments/assets/66043696-7697-480f-a6ff-4de1197d649f)

_Figure 6: mesh sensitivity analysis_

| element size (m) | # elements | temp (°C) |
|------------------|------------|-----------|
| 0.2              | 544        | 18.771    |
| 0.15             | 925        | 19.648    |
| 0.1              | 2522       | 19.684    |
| 0.085            | 3950       | 19.835    |
| 0.07             | 6427       | 19.803    |
| 0.055            | 12766      | 19.803    |
| 0.04             | 30801      | 19.846    |
| 0.03             | 64559      | 19.779    |
| 0.024            | 117264     | 19.821    |
| 0.018            | 225796     | 19.881    |

_Table 1: sensitivity mesh analysis_






## Temperature Distribution of the Freezer and Wine Bottles

The temperature distribution within the freezer and wine bottles was analyzed at three distinct time points: \( t = 600 \, s \), \( t = 23600 \, s \), and \( t = 43200 \, s \). Both vertical and horizontal contour plots were created to showcase how the temperature gradients evolve over time within the system.

### Temperature at \( t = 600 \, s \)

At \( t = 600 \, s \), the evaporator induces large temperature gradients, with the bulk fluid cooling from \( 2^\circ C \) to \( -6^\circ C \). A higher temperature of \( 6^\circ C \) is observed at the front edge. The wine bottles are connected to a \( 2^\circ C \) fluid, which gradually cools and gets replaced by bulk fluid. Small air pockets with temperatures in the range of \( -6^\circ C \) to \( -10^\circ C \) begin to form.

### Temperature at \( t = 23600 \, s \)

By \( t = 23600 \, s \), the wine bottle radiates its heat energy towards the freezer. This creates additional temperature gradients, with the evaporator actively cooling the freezer’s bulk fluid. The wine bottles cool to about \( 14^\circ C \), and the connecting warmer fluid is replaced by bulk fluid. The freezer's bulk fluid now ranges between \( -10^\circ C \) and \( -15^\circ C \), with larger cold air pockets at the front. The edges of the wine bottles show larger temperature gradients, reaching up to \( 6.5^\circ C \) higher compared to the bulk fluid.

### Temperature at \( t = 43200 \, s \)

At \( t = 43200 \, s \), the wine bottles reach their final temperature of about \( 8^\circ C \). The temperature difference between the bulk fluid and the wine bottles’ edges ranges from \( 4^\circ C \) to \( 5^\circ C \). The bottom of the freezer remains colder at around \( -15^\circ C \), while the top reaches \( -10^\circ C \). This temperature difference explains why the bottom wine bottles exhibit a lower volume average temperature compared to the top bottles.

### Vertical Temperature Distribution

The following figure displays the vertical temperature distribution along the YZ-plane at \( x = 0.25 \, m \) for the three time points. The contour plots highlight how the temperature gradually spreads vertically across the wine bottles and surrounding freezer environment.

![image](https://github.com/user-attachments/assets/085202d3-8732-4930-aecf-bc0d90f26aa5)

_Figure 7: Vertical temperature distribution contour plot on the YZ-plane at \( x = 0.25 \, m \) for \( t = 600 \, s \), \( t = 23600 \, s \), and \( t = 43200 \, s \)._

### Horizontal Temperature Distribution

Similarly, the horizontal temperature distribution along the XZ-plane at \( y = 0.7 \, m \) demonstrates how heat is transferred horizontally through the system. The plots at \( t = 600 \, s \), \( t = 23600 \, s \), and \( t = 43200 \, s \) reveal the varying thermal gradients at different points in time, with a more pronounced difference near the wine bottles' edges.

![image](https://github.com/user-attachments/assets/da2f8923-a115-4472-b74b-8ebda1986276)

_Figure 8: Horizontal temperature distribution contour plot on the XZ-plane at \( y = 0.7 \, m \) for \( t = 600 \, s \), \( t = 23600 \, s \), and \( t = 43200 \, s \)._






## Volume Average Temperature of Wine Bottles at 6 o'clock

Bart's colleagues will arrive at 6 o'clock in the afternoon, and the average temperature of the different wine bottles must be determined. The volume average temperature of each bottle was plotted as a function of time to observe its evolution, with the results shown in Figure 9.

### Temperature Evolution Over Time

The plot in Figure 9 demonstrates how the wine bottles, which all started at an initial temperature of \( 20^\circ C \), cooled over time. The wine bottle in the bottom right (next to the evaporator) exhibited the strongest decrease in temperature. Interestingly, the bottom left wine bottles cooled faster than the top right ones, which are closer to the evaporator. This effect may be attributed to the direction of the convective flow, which will be further examined in the velocity profile analysis (Question 6). The top left bottle had the slowest temperature decrease overall.

### Volume Average Temperature at 6 o'clock

At 6 o'clock, the volume average temperature for each bottle was recorded, and an overall volume average temperature of \( 14.03^\circ C \) was obtained, as shown in Table 2.

| wine bottle location | Volume Average Temperature (°C) |
|----------------------|----------------------------------|
| right top            | 14.27                           |
| right bottom         | 13.45                           |
| left top             | 14.55                           |
| left bottom          | 13.84                           |
| all                  | 14.03                           |

_Table 2: Volume average temperature at 6 o'clock in \( ^\circ C \)._

### Evolution of Volume Average Temperature

The graph below shows the evolution of the volume average temperature for each bottle over time. The bottom right bottle, being closest to the evaporator, cools the fastest, while the top left bottle cools the slowest.

![image](https://github.com/user-attachments/assets/d975cca9-15ba-4e63-9842-699e27415f5c)

_Figure 9: Volume average temperature \([^\circ C]\) vs time \([s]\)._




## Vector Plots of Air Flow in the Freezer

The air flow inside the freezer was visualized using vector plots at three different times: \( t = 600 \, s \), \( t = 23600 \, s \), and \( t = 43200 \, s \). These plots help illustrate the convective flow of air around the wine bottles and the surrounding fluid as it cools down.

### Air Flow at \( t = 600 \, s \)

At the start of the simulation, the fluid near the evaporator cools rapidly, causing it to become denser and sink to the bottom of the freezer. This results in high air velocities near the evaporator wall, as shown in Figure 12. As the fluid cools, it spreads along the bottom of the freezer, while warmer air rises towards the top, creating a circulating flow.

### Air Flow at \( t = 23600 \, s \)

As the freezer cools, the air flow velocity decreases and the system starts to reach a more steady state. The high temperature gradients from the initial stage have reduced, which leads to slower convective flows, as illustrated in Figures 10 and 11.

### Air Flow at \( t = 43200 \, s \)

By the final time point, the temperature inside the freezer is more uniform, and the air flow velocities are much lower. The air circulation is still present but has significantly diminished compared to the earlier time points, as seen in the vector plots.

### Vector Plots of Top and Bottom Wine Bottles

The vector plots of the top and bottom wine bottles (Figures 10 and 11) clearly demonstrate how the velocity gradients differ between the bottles. The bottom wine bottles exhibit larger velocity gradients due to their proximity to the evaporator, where the temperature difference is most significant. The vector plot in Figure 10 for the top bottles shows less dense air velocity vectors compared to the bottom bottles (Figure 11), which experience faster convective currents near the bottom right.

### Vector Plot in the YZ-Plane

The vector plot in the YZ-plane (Figure 12) shows how the air flows along the evaporator wall and across the freezer at different time points. At \( t = 600 \, s \), the air movement is more pronounced as it cools and descends. Over time, the flow stabilizes, and the velocity of the air decreases significantly by \( t = 43200 \, s \).

### 3D Visualization of Air Flow

A 3D vector plot (Figure 13) was created to visualize the air flow around the wine bottles from a different perspective. This plot offers insight into how the fluid circulates around the bottles, especially at \( t = 600 \, s \), when the convective flow is at its peak.

![image](https://github.com/user-attachments/assets/5610949f-9187-41d4-b723-2eff9d2be4d9)

_Figure 10: Vector plot of top wine bottles on XZ-plane at \( y = 0.7 \, m \) at \( t = 600 \, s \), \( t = 23600 \, s \), and \( t = 43200 \, s \)._

![image](https://github.com/user-attachments/assets/e6fa5d18-2273-4c85-a1cb-4d8600349c1c)

_Figure 11: Vector plot of bottom wine bottles on XZ-plane at \( y = 0.25 \, m \) at \( t = 600 \, s \), \( t = 23600 \, s \), and \( t = 43200 \, s \)._

![image](https://github.com/user-attachments/assets/e2d231b3-b7e9-40db-864e-a7ae2518f328)
![image](https://github.com/user-attachments/assets/5e831215-c3b2-4183-b198-ed5d89a8e0b9)
![image](https://github.com/user-attachments/assets/ac54a7b4-36ec-45db-a546-4ecdb0a240e4)

_Figure 12: Vector plot on YZ-plane at \( y = 0.7 \, m \) at \( t = 600 \, s \), \( t = 23600 \, s \), and \( t = 43200 \, s \)._

![image](https://github.com/user-attachments/assets/931ae153-bb77-4b78-95ef-ed06192b02ce)

_Figure 13: 3D visualization of vector plots of wine bottles on XZ-plane at \( y = 0.25 \, m \) and \( y = 0.70 \, m \) at \( t = 600 \, s \)._





## Ratio of Incoming Heat Flow Over Outgoing Heat Flow

In this section, we calculate the ratio of the incoming heat flow to the outgoing heat flow. The heat flux is defined as the heat flow per unit area and is given by the equation:

$$
\text{Heat flux} = \frac{\text{Heat flow}}{\text{Area}}
$$

Rearranging this yields the equation for heat flow:

$$
\text{Heat flow} = \text{Heat flux} \times \text{Area}
$$

The incoming heat flow represents all the heat entering the control volume, primarily through the surrounding air. Since the outside air is warmer than the freezer, heat flows into the freezer. Additionally, the wine bottles, being at a higher temperature than the air inside the freezer, contribute to heat flow into the system. The outgoing heat flow is driven by the evaporator, which is the only surface removing heat from the freezer.

### Heat Flow Calculation at \( t = 21{,}600 \, \text{s} \)

At \( t = 21{,}600 \, \text{s} \), the heat flow was calculated using Ansys with the following expression:

$$
\frac{
\left[ \text{areaAve}(\text{HeatFlux}) @ \text{left face} \times 0.605 \right] + 
\left[ \text{areaAve}(\text{HeatFlux}) @ \text{right face} \times 0.605 \right] + 
\left[ \text{areaAve}(\text{HeatFlux}) @ \text{front face} \times 0.55 \right] + 
\left[ \text{areaAve}(\text{HeatFlux}) @ \text{top face} \times 0.275 \right] + 
\left[ \text{areaAve}(\text{HeatFlux}) @ \text{wine bottle faces} \times 1.256 \right]
}{
\text{areaAve}(\text{HeatFlux}) @ \text{back face} \times 0.55
}
$$

The areas of the different faces were multiplied by their respective heat flux values to calculate the heat flow through each face. The area of the wine bottles was calculated as:

$$
A_{\text{wine bottles}} = 4 \times \left( 2 \pi r h + 2 \pi r^2 \right)
$$

Substituting the radius and height values:

$$
A_{\text{wine bottles}} = 4 \times \left( 2 \pi (0.1\, \text{m})(0.4\, \text{m}) + 2 \pi (0.1\, \text{m})^2 \right)
$$

Simplifying further:

$$
A_{\text{wine bottles}} = 4 \times \left(0.2513\, \text{m}^2 + 0.0628\, \text{m}^2\right) = 1.2564\, \text{m}^2
$$

This results in a ratio of incoming heat flow to outgoing heat flow:

$$
\frac{\text{Incoming heat flow}}{\text{Outgoing heat flow}} = 0.1397 = \frac{139}{1000}
$$

### Interpretation

The ratio indicates that the evaporator is removing more heat than is entering the system. This is consistent with the temperature profiles discussed in Question 4, where the freezer's bulk fluid cools down over time. The evaporator effectively manages the heat flow, ensuring that the freezer remains colder than the surrounding environment.











