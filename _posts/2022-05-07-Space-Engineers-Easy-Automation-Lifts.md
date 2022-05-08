---
layout: post
title:  "Easy Automation V2.0 Lifts"
date:   2022-05-07 00:00:00 +0000
categories: [Games, Space Engineers]
author: Joe
wip: true
---
{% if page.wip %}
## Page under construction
{% endif %}

## Purpose:

Minimum build steps for creating an aligned multi-level lift-platform in Space Engineers using Coren's [Easy Automation V2.0] in-game script.<!-- more -->

## Requirements:

* In-game scripts allowed
* Subscribe to Coren's [Easy Automation V2.0]
* Piston-type block
  * Demo'd using 不言丶's [Piston Elevators] mod
* Programmable Block
* Button panel(s)

## Optional:

* LCDs

## Steps:

1. Prep the lift area, drill any holes needed etc.
  * The cross-section of the platform will need to be able to pass through the usable length of the shaft.
  * A service area for a horizontal piston will need to be cleared either at the top or bottom of the shaft (depending on whether you are pushing or pulling the lift).
1. Place a horizontal piston such that an armour block on the head will be directly underneath the platform and that the base is at the 'front' of the platform.
  * This shifts the collision boxes of the grids enough that they will slide past each other on the front side.
  * See [Splitsie's Elevator Vanilla Build][Splitsie] video for a detailed demonstration.
1. The horizontal piston can be turned off immediately.
1. Add the armour block to the end of the piston and place the (first) vertical piston.
  * See other guides on arranging pistons with blast doors to stack multiple pistons in a compact space.
  * Vanilla Pistons can be stacked to get extra reach, or a mod can be used to add extended pistons (e.g. 不言丶's [Piston Elevators] mod).
1. Build up the vertical piston(s) and platform as you see fit, such that the platform can be extended/retracted through the full distance of the lift shaft.
1. Find and note down the optimal stopping distances for every level needed for the lift.
  * \[Control]+\[Left Click] the maximum/minimum distance sliders to get fine control.
  * **ToDo: workout method for using multiple vertical pistons.**
1. Note the optimal distances to 1 decimal point.
1. Place a Programmable Block somewhere on the grid and load the [Easy Automation V2.0] script from the workshop.
1. In a suitable permanent block with Custom Data, add the below script to the **Custom Data** section.
  * I've used the Horizontal Piston to store the script as this should not need to be used or changed after it's built.
1. Build Button and LCD panels at each level.
1. For your own sanity I recommend renaming all the blocks being used using any scheme you are comfortable with.
  * I've prefixed all my blocks with the Grid name, followed by their function and a number suffix if there are multiples (e.g. multiple vertical pistons).
  * E.g. *"\[Luna] Lift Piston H"* for the horizontal piston of the Luna lift assembly.
1. Edit the Custom Data script to use your own distances (both exact and rounded) in the `@Variables{}` section.
  * See below for examples and explanation.
1. Set a Button Action for each level with the below settings:
  * Using the [Easy Automation V2.0] Programmable Block
  * *'Run'*
  * Argument: `Lift Piston H(Lift(LiftFloor0, LiftFloor0Exact, Garage))`  
  Where `Lift Piston H()` is where the Custom Data script is stored  
  `Lift()` is the function
  `LiftFloor0, LiftFloor0Exact, Garage` are the arguments to be used for the Lift() function - these should be changed per-button depending on the level the button is for, see below.

# Script:

Blocks:
* *"Lift Maglev Piston"* - Vertical piston
* *"Lift LCDs"* - Group of LCD panels for displaying lift status
* *"Lift Piston H"* - Horizontal piston for storing script

Variables:
* LiftFloorXExact = 0.000 - exact position you want the piston to stop at for floor X
* LiftFloorX = 0.0 - position above rounded to 1 decimal place
* LiftVelocity = 2 - positive velocity
* LiftVelocityNeg = -2 - negative velocity

```
@Variables{
    LiftFloor2 = 50.1
    LiftFloor2Exact = 50.100
    LiftFloor1 = 27.5
    LiftFloor1Exact = 27.500
    LiftFloor0 = 0.0
    LiftFloor0Exact = 0.000
    LiftVelocity = 2
    LiftVelocityNeg = -2
}

@Lift{
    WriteNew to (Lift LCDs) = "Moving to"
    WriteLine to (Lift LCDs) = "Hanger" *3
    If Current position of Lift Maglev Piston < LiftFloor1 *1
    {
        UpperLimit of Lift Maglev Piston = LiftFloor1Exact *2
        LowerLimit of Lift Maglev Piston = LiftFloor0Exact
        Velocity of Lift Maglev Piston = LiftVelocity
    }
    Else If Current position of Lift Maglev Piston > LiftFloor1 *1
    {
        LowerLimit of Lift Maglev Piston = LiftFloor1Exact *2
        UpperLimit of Lift Maglev Piston = LiftFloor2Exact
        Velocity of Lift Maglev Piston = LiftVelocityNeg
    }
    Else If Current position of Lift Maglev Piston = LiftFloor1 *1
    {
        WriteLine to (Lift LCDs) = "Level"
    }
    Delay 10000
    WriteNew to (Lift LCDs) = "Hanger" *3
}
```

# Explanation:

See the [Easy Automation Guide] for more details.  
**Variables:**  
Each floor has two positions, the exact and the rounded version. This is because the Position value reported by Easy Automation comes from the Detailed Info section of the Control Panel for the piston, which is rounded to 1 decimal place. This rounded value is what we will index against to see whether the lift is above or below the target level, not the actual distance.  
My lift assembly is indexed to zero (0)m by using another vertical piston to fine-tune the ground floor resting position, but your mileage may vary.

The lift velocity can be tweaked here as well

**Lift:**  
`WriteNew to (Lift LCDs) = "Moving to"` clears LCDs in the *"Lift LCDs"* group and then writes 'Moving to' to the screens.  
`WriteLine to (Lift LCDs) = "Hanger" *3` adds a new line with the default text 'Hanger'. The `*3` at the end allows us to replace this text with different text later.  
`If Current position of Lift Maglev Piston < LiftFloor1 *1` checks if the current position of the piston is *below* the LiftFloor1 value (or a substituted value `*1`) then...  
  * sets the `UpperLimit` (Max Distance) of the piston to the desired level, default `LiftFloor1Exact` or a substituted value `*2`  
  * sets the `LowerLimit` (Min Distance) of the piston to the lowest level  
  * starts the piston extending upwards

`Else If Current position of Lift Maglev Piston > LiftFloor1 *1` checks if the current position of the piston is *above* the LiftFloor1 value (or a substituted value `*1`) then...  
  * sets the `LowerLimit` (Min Distance) of the piston to the desired level, default `LiftFloor1Exact` or a substituted value `*2`  
  * sets the `UpperLimit` (Max Distance) of the piston to the highest level  
  * starts the piston retracting downwards  

`Delay 10000` waits 10s (10000ms) and finally...  
`WriteNew to (Lift LCDs) = "Hanger" *3` replaces the LCD screen text with 'Hanger' or the substituted text in `*3`.

**\*X:**
Used to substitute arguments from the activating device (e.g. button) into the script.  
If you're familiar with Batch scripting this is very similar, otherwise where you see `*1` at the end of a line, the previous variable (`LiftFloor1`) will be replaced by the first argument in the innermost brackets in the button's Run argument.  
`*1` => *Lift Piston H(Lift(****LiftFloor0****, LiftFloor0Exact, Garage))*  
`*2` will be the second argument etc.  
This argument can be another variable or an explicit value, like `*3` which is the text '*Garage*'

# Example Button Actions:

**Floor 0/Garage:** `Lift Piston H(Lift(LiftFloor0, LiftFloor0Exact, Garage))`  
**Floor 1/Hangar:** `Lift Piston H(Lift(LiftFloor1, LiftFloor1Exact, Hanger))`  
**Floor 2/Engineering:** `Lift Piston H(Lift(LiftFloor2, LiftFloor2Exact, Engineering))`  


[Easy Automation V2.0]: https://steamcommunity.com/sharedfiles/filedetails/?id=694296356
[Easy Automation Guide]: https://steamcommunity.com/sharedfiles/filedetails/?id=685206874
[Piston Elevators]: https://steamcommunity.com/sharedfiles/filedetails/?id=2580360867
[Splitsie]: https://www.youtube.com/watch?v=z8-qgbCXsIY