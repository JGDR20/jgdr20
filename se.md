---
layout: page
title: Space Engineers
# permalink: /se/index.html
sidebar: No
---
## Game Servers

### Private
Contact me to request an invite

| # 	| Mode 	| Scenario 	| Description | Connection 	| Slots 	| Status	 |
|-	|-	|-	|-	|-	| -	 |
| 1 	| Survival 	| Star System 	| Modded survival with <a href="https://steamcommunity.com/sharedfiles/filedetails/?id=2847055633">Assertive Acquisitions collection</a> from Splitsie, plus a few others | <a href="steam://connect/176.31.228.98:29314">176.31.228.98:29314</a> 	| 10 	| <span style="color:#00b300;">Online</span>	 |
| 2 	| Survival 	| Earth-Like 	| Rovers only | <a href="steam://connect/51.38.93.181:29016">51.38.93.181:29016</a> 	| 10 	| <span style="color:#b3b3b3;">Held</span>	 |

### Colour Scheme

#### JGDR20
<span style="color:#FF0097;">Main: #FF0097 / 255, 0, 151</span>  
<span style="color:#00B3FF;">Accent: #00B3FF / 0, 179, 255</span>  

### Belaros
<span style="color:#ff6700;">Main: #FF6700 / 255, 103, 0</span>  
<span style="color:#CCFF00; background-color: #505050;">Bumblebee: #CCFF00 / 80, 100, 0</span>  

### Hexcaster
<span style="color:#000000;">Main: #000000 / 0, 0, 0</span>  

### MagmaNoggin
<span style="color:#b30000;">Main: #b30000 / 179, 0, 0</span>  

### Ice Base
White sci-fi armour  
<span style="color:#FFFFFF; background-color:#505050;">Main: #FFFFFF / 255, 255, 255</span>  

## Blueprints
### Welder Projector
#### Tick Mk I

| Dimension | Value |
| - | - |
| H | 0 |
| V | 1 |
| F | 19 |
| P | 90 |
| Y | 0 |
| R | 0 |
| S | 100 |

#### Tick Mk II

| Dimension | Value |
| -	 | -	 |
| H | 2 |
| V | 0 |
| F | 5 |
| P | -90 |
| Y | 0 |
| R | 0 |
| S | 100 |

| # | Block |
| - | - |
| 195 | Total |
| 169 | Light Armour |
| 2 | Left Offroad Suspension (5x5) |
| 1 | Connector |
| 1 | Small Curved Conveyor Tube |
| 1 | Antenna |
| 2 | Right Offroad Suspension (5x5) |
| 2 | Interior Light |
| 3 | Timer Block |
| 1 | Medium Cargo |
| 2 | Programmable Blocks |
| 2 | Gyroscopes |
| 2 | Rotors |
| 2 | Text Panels |
| (1) | (Projector) |
| 1 | Gattling Turret |
| 1 | Battery |
| 1 | Spotlight |
| 1 | Fighter Cockpit |

#### Lil' Blue Friend

| Dimension | Value |
| - | - |
| H | 1 |
| V | 1 |
| F | 8 |
| P | 0 |
| Y | 0 |
| R | 0 |
| S | 100 |

| # | Block |
| - | - |
| 74 | Total |
| 53 | Light Armour |
| 10 | Atmospheric Thruster (Sci-Fi) |
| 1 | Connector |
| 1 | Antenna |
| 1 | Programmable Block |
| 2 | Gyroscope |
| 3 | Battery |
| 1 | Fighter Cockpit |
| 1 | Ore Detector |
| 1 | Landing Gear |

#### Lil' Blue Friend Mk II
(Self)

| Dimension | Value |
| - | - |
| H | 0 |
| V | 5 |
| F | 5 |
| P | -90 |
| Y | 0 |
| R | 0 |
| S | 100 |

| # | Block |
| - | - |
| 167 | Total |
| 53 | Light Armour |
| 10 | Atmospheric Thruster (Sci-Fi) |
| 1 | Connector |
| 1 | Antenna |
| 1 | Programmable Block |
| 2 | Gyroscope |
| 3 | Battery |
| 1 | Fighter Cockpit |
| 1 | Ore Detector |
| 1 | Landing Gear |

### SAMv2 Config

#### Hive

##### Base Programmable Block
SAM.
SAM.ADVERTISE
SAM.Name=Hive

#### Maw
##### Base Programmable Block
SAM.
SAM.ADVERTISE
SAM.Name=Maw

#### Whale Connector 2
SAM.
SAM.Name=WhaleConnector

### Dotty Prog Blocks, Timers etc.

#### Dotty Prog [SAM]

| Script | Block Type | Block Name | Custom Data/Other Changes |
| - | - | - | - |
| Sam's Autopilot Manager V2 | Programmable Block | Dotty Prog \[SAM\] | <strong>Custom Data:</strong><br><code>SAM.<br>SAM.ApproachDistance=100<br>SAM.ApproachingSpeed=5<br>SAM.DockingSpeed=1<br>SAM.TaxiingSpeed=2<br>SAM.TaxiingPanelDistance=5<br>SAM.NODAMPENERS</code> |
| " | Remote Control | Dotty Remote Control | <strong>Custom Data:</strong><br><code>SAM.</code> |
| " | Antenna | Dotty Antenna | <strong>Custom Data:</strong><br><code>SAM.</code> |
| " | Transparent LCD Panel | Dotty Transparent LCD L1 SAM | <strong>Custom Data:</strong><br><code>SAM.</code> |
| " | " | Dotty Transparent LCD L2 SAM Instructions | <strong>Text (with white space):</strong><br><code>   CTRL+2<br>   1: Screen   3: Next<br>   2: Prev      4: Select<br>                      5: Start<br>                      6: Stop<br><br><br>    7: Add<br>    8: Add Stance<br>    9: Remove</code><br>Font Size: 1.750 |
| " | Transparent LCD Panel | Dotty Transparent LCD R1 SAM Log | <strong>Custom Data:</strong><br><code>SAM.Log</code> |
| " | Camera | Dotty Camera F/B/U/D/L/R | <strong>Custom Data:</strong><br><code>SAM.</code> |
| " | Connector | Dotty Connector | Ship<br><strong>Custom Data:</strong><br><code>SAM.<br>SAM.Name=Dotty-Connector</code> |
| " | " | Maw Dotty Connector | Station<br><strong>Custom Data:</strong><br><code>SAM.<br>SAM.Name=Dotty</code> |
| " | LCD Panel | Maw Dotty Taxi LCD 0.. \[Start/End\] | <strong>Custom Data:</strong><br><code>SAM.<br>SAM.Name=Dotty</code><br>Texture: Arrow |
| \[PAM\] - Path Auto Miner | Programmable Block | Dotty Prog \[PAMDotty\] | <strong>Code:</strong><br><code>    const String pamTag = "[PAMDotty]";<br>    const String controllerTag = "[PAM-Controller-Dotty]";</code><br><br><strong>Custom Data:</strong><br><code>[PAM-Ship] Broadcast-settings<br><br>Enable_Broadcast=true<br>Ship_Name=Dotty<br>Broadcast_Channel=#Dotty</code> |
| " | Transparent LCD Panel | Dotty Transparent LCD R4 \[PAMDotty\] | N/A |
| " | " | Dotty Transparent LCD R3 PAM Instructions | <strong>Text (with white space):</strong><br><code><br><br><br>    CTRL+3<br>    1: Up<br>    2: Apply<br>    3: Down<br><br>    5: Align</code><br>Font Size: 1.750 |
| " | Sensor | Dotty Sensor Down \[PAMDotty\] | N/A |
| Inventory Content Display Script | Programmable Block | Dotty Prog Inventory | N/A |
| " | Transparent LCD Panel | Dotty Transparent LCD L3/L4/C Inventory1/2/3 | <strong>Public Name:</strong><br>Res #0/1/2 |
| Isy's Ship Refueler | Programmable Block | Dotty Prog Refuel | <strong>Code:</strong><br><code>// -- Timer trigger on event --<br>...<br>string\[\] events = { "dock", "undock" };<br>string\[\] timers = { "Dotty Timer Dock", "Dotty Timer Undock" }; |
| " | Timer | Dotty Timer Dock | <strong>Actions:</strong><br>Dotty Atmo: Off<br>Dotty Hydro Thrusters: Off<br>Dotty Ions: Off |
| " | Timer | Dotty Timer Undock | <strong>Actions:</strong><br>Dotty Atmo: On<br>Dotty Hydro Thrusters: On<br>Dotty Ions: On |
| " | Transparent LCD Panel | Dotty Transparent LCD R2 \[IsyLCD\] | <strong>Custom Data:</strong><br><code>ISR-main</code> |




### Ideas

* 'Flippable' mobile processor
  * Independent wheels on roof
* Rescue/crane unit
  * Rotors and pistons to landing gear
  * Lift damaged/turtled craft, rotate and set back down
* Mobile power plant
  * 9x9 grid
  * Wind tubines raised on corners
  * Piston+Landing gear to become 'stationary' on-site, retract for movement
  
Steam Group ID 103582791469779084