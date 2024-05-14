# Modified-OpenTorque-Project

## Table of Contents

1. [Introduction](#1-introduction)  
2. [Methodology](#2-methodology)
- 2.1. [3D-Printing Methodology](#31-3d-printing-methodology)
- 2.2. [ODrive Methodology](#32-odrive-methodology)
- 2.3. [Physical Testing Methodology](#33-physical-testing-methodology)
3. [Results](#3-results)
4. [Future Work](#4-future-work)
5. [References](#5-references)
6. [Acknowledgements](#6-acknowledgements)

## 1. Introduction
The OpenTorque project [1] is a robotic actuator developed for legged robots originally by Gabrael Levine and updated [2] by user acisre. The construction of the actuator involves a 100KV brushless DC motor, motor controller, 3D-printed carbon-fiber enclosure, and planetary gearbox. According to the original creator, the actuator performance is summarized in the table below:

| Amperage | Holding Torque (Nm) |
| ------------- | ------------- |
| 10  | 7 |
| 30 | 14  |
| 60 | 28  |

According to tests performed by another user Skyentific [3], the actuator performance is summarized in the table below:

| Amperage | Holding Torque (Nm) |
| ------------- | ------------- |
| 10  | 6.5727 |
| 20 | 13.2435 |
| 30 | 19.1295 |
| 40 | 25.3098 |

Additionally, for both tests, the motor was able to hold 20 lbs on a 12-inch lever arm.Due to the age of the OpenTorque project, many original parts have been discontinued, therefore parts have been replaced. Parts which have been replaced include the original motor, encoder, and motor controller. The picture below displays the CAD model of the newly modified OpenTorque actuator:

![Modified-OpenTorque-Project_1](/Modified-OpenTorque-Project_IMGs/Modified-OpenTorque-Project_1.PNG)

Due to the modified structure, motor, encoder, and controller, the actuator must be retested to determine the actual torque output of the actuator. 

## 2. Methodology
## 2.1. 3D-Printing Methodology
Robotic actuators generally experience great force and strain during operation, therefore strong materials should be used during manufacturing to ensure components do not break during operation. To keep the original vision of the original creator (3D-printed parts), and because the construction of the gearbox and housing is complex, carbon-fiber (CF) based filaments were applied during the manufacturing of the modified OpenTorque actuator. The table below summarizes manufacturer characteristics [4] of all applied CF based filaments compared to standard PLA:

| | PLA | PLA-CF | PA6-CF |
| --- | ------------- | ------------- | ------------- |
| Impact Strength – XY (kJ/m^2)  | 26.6 | 23.2 | 40.3 |
| Bending Strength – XY (MPa) | 76 | 89 | 151 |
| Bending Modulus – XY (MPa) | 2750 | 3950 | 5460 |
| Impact Strength – Z (kJ/m^2) | 13.8 | 7.8 | 15.5 |

CF based filaments are generally stronger than standard filaments. Since 3D-printed components and moving gearboxes are difficult to conduct force simulations upon, the following decision tree was applied when selecting filaments for components:

![Modified-OpenTorque-Project_2](/Modified-OpenTorque-Project_IMGs/Modified-OpenTorque-Project_2.PNG)

Generally, with moving components, the toughest filaments are advisable to prevent wear during continuous use. Wear may cause the moving components (gearbox) to fail and may lead to reduced actuator performance. Tougher filaments (PLA-CF) are advisable on structural but not moving components to ensure stiffness. PLA is acceptable on electronic or cosmetic components to save cost since these parts do not generally experience force and strain. All 3D-print settings and components are included within the given 3MF files. 

## 2.2. ODrive Methodology
Robotic actuators require a controller and encoder to control the position, velocity, and torque of the actuator. The chosen controller-encoder combo is the ODrive S1. The ODrive S1 [5] is an accurate FOC controller capable of control with ROS or Arduino MCUs, and natively uses position, velocity, and torque control. During the construction of the modified OpenTorque actuator, 48V was applied DC- and DC+ using a MEAN WELL LRS-350-48 dedicated power supply. Pins A, B, and C were connected to the respective wire on the chosen 100KV motor (*note, where plug in does not matter). Thermistor wires from the motor were connected to the corresponding pins on the ODrive S1. R- and R+ were connected to opposite ends of the given brake resistor. To connect the ODrive S1 for programming, a USB isolator was used to prevent accidental electrical damage to a programming computer. 

Once physically connected, the ODrive S1 was connected to the ODrive GUI. The following settings were applied to safely operate the actuator:

![Modified-OpenTorque-Project_3](/Modified-OpenTorque-Project_IMGs/Modified-OpenTorque-Project_3.PNG)
![Modified-OpenTorque-Project_4](/Modified-OpenTorque-Project_IMGs/Modified-OpenTorque-Project_4.PNG)
![Modified-OpenTorque-Project_5](/Modified-OpenTorque-Project_IMGs/Modified-OpenTorque-Project_5.PNG)

All settings are applied to safely operate the actuator. Once settings were set, a calibration sequence was performed, and the dashboard was accessed to manually control position, velocity, and torque. Removing safety will likely increase the performance of the actuator. 

## 2.3. Physical Testing Methodology
To test the maximum torque output of the modified OpenTorque actuator, force must be applied to the center of a scale set to kg by an arm of known length. Force may be applied using the position or torque control functionality through the dashboard. The following equation should be used to calculate the torque output of the OpenTorque actuator:

9.81  *  scale_reading(kg)  *  arm_length(m)  =  torque(Nm)

The following experimental setup was used to determine torque:

![Modified-OpenTorque-Project_7](/Modified-OpenTorque-Project_IMGs/Modified-OpenTorque-Project_7.PNG)

## 3. Results
Based on torque testing, the following values were obtained:

| Amperage | Holding Torque (Nm) |
| ------------- | ------------- |
| 42.777  | 24.7 |
| 80 | ??? |

| Amperage | Gabrael Levine OpenTorque | Skyentific OpenTorque | Modified OpenTorque |
| ------------- | ------------- | ------------- | ------------- |
| 10 | 7 | 	6.5727	| - | 
| 20 | - | 13.2435 | - | 
| 30 | 	14 | 19.1295 | - | 
| 40 | - | 25.3098 | 24.7 | 
| 50 | - | - | - | 
| 60 | 	28 | - | - | 
| 70 | - | - | - | 
| 80 | - | - | ??? | 

The picture displays the position, velocity, and current graphs during the maximum torque test using position control. The limit of approximately 40A was reached during the test, which is displayed in the current vs time graph. If “DC bus overvoltage trip-level” was set to 24V instead of 50V and a 24V supply was used instead of a 48V supply, the internal current limit would increase to 80A and likely increase maximum torque while reducing velocity. Additionally, if the torque was not limited to 20% of the maximum value, the torque value could increase when operating using torque control mode. 

![Modified-OpenTorque-Project_8](/Modified-OpenTorque-Project_IMGs/Modified-OpenTorque-Project_8.PNG)

## 4. Future Work
Future tests of this actuator should involve implementing a 24V supply to increase the current limit to 80A. Implementing the increased amperage should involve temperature tests during operation, since the motor will likely increase in temperature, causing a reduction in performance. 

## 5. References
[1] https://hackaday.io/project/159404-opentorque-actuator

[2] https://discourse.odriverobotics.com/t/opentorque-project-new-design/8280

[3] https://www.youtube.com/watch?v=6lW2YGQQIQ4

[4] https://bambulab.com/en-us/filament-guide

[5] https://docs.odriverobotics.com/v/latest/hardware/s1-datasheet.html

## 6. Acknowledgements

## License
[MIT](https://choosealicense.com/licenses/mit/)

