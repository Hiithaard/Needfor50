# Need for 50: Race Against Time (Final Assignment)
## Overview
So here in my final assignment, I have tried to create a clone of the famed racing game franchise Need for Speed. Need for Speed is EA's one of the highest-grossing franchises, stacking up a whopping $2.7 billion in sales since its first release in 1994.

The original game mainly focuses on illegal street racing against real or AI opponents, often in urban environments. **However, in my version**, I have created a simple race in which the player has to cross the finish line before they run out of time. Both the [car model](https://assetstore.unity.com/packages/3d/vehicles/race-car-package-141690) and the [race track](https://assetstore.unity.com/packages/3d/environments/roadways/race-tracks-140501) have been attained from the Asset Store.

## Explanation of the game

### Car

The main car object in the game is a **[Rigidbody](https://docs.unity3d.com/ScriptReference/Rigidbody.html)** has three children: **Wheel Transforms**, **Wheel Colliders** and **Lights**

#### Colliders and Transforms

Colliders make the car move by interacting with the ground and rotating while the transforms mimic the position and rotation of the colliders so that it looks like the wheels are actually moving.

The arrow keys are used to control the car.

When the **up** arrow is pressed, [motor torque](https://docs.unity3d.com/ScriptReference/WheelCollider-motorTorque.html) is applied to the wheel colliders. When the **down arrow** or the **space bar** is pressed, [brake torque](https://docs.unity3d.com/ScriptReference/WheelCollider-brakeTorque.html) and [drag](https://docs.unity3d.com/ScriptReference/Rigidbody-drag.html) are applied. *Drag is used because the brake torque alone was not sufficient to slow down the car.*

The **left** and **right** arrow keys change the direction of the front wheels for turning.

#### Lights

Since I have set the race to occur at night, I have implemented lights in the car object. It has four lights in total, two spotlights (headlights) and two-point lights (tail lights). *See: [Types of lights](https://docs.unity3d.com/Manual/Lighting.html)*

The tail lights glow brighter when braking is applied. Initially, the [intensity](https://docs.unity3d.com/Manual/Lighting.html) of tail lights is set to 0.1, but when brakes are applied, it is increased to 1. This is achieved using a **braking** flag.

When the braking flag is true *(down arrow or space bar is pressed)* the intensity is increased to 1, when braking flag is false *(down arrow or space bar is not pressed)* the intensity is set back to 0.1.

The speed of the car is displayed at the top left corner of the screen using **Rigidbody.velocity.magnitude**.

### Camera follow (Script)

This script is assigned to the main camera of the game. Takes in the car as the target (type: Rigidbody) and interpolates the camera's position according to it.

There are just two functions in this script, **one handles camera translation**, following the car wherever it goes, and the other **handles camera rotation**, rotating the camera according to the car.

There are two float variables called **translateSpeed** and **rotationSpeed**, which are the speeds at which the interpolation occurs. The higher their value, the more rigid camera movement will occur. Lower they are, slower but smoother movement will occur.

There is another vector3 variable called **offset**, these are the values by which the camera is offset to get a better angle at the car. Without the offset, the camera would be at a very awkward angle, making it difficult to see the car.

### Timer

To win the game, the player has to reach the end of the track within **4 minutes and 30 seconds**.

There is a variable that stores the total time available in seconds (270 seconds). Then in the update function, the total time is decremented by Time.deltaTime (time between two frames).

Minutes are calculated by dividing the total time remaining by 60 (total_time/60) and seconds are calculated using the modulus operator (total_time % 60). Both the values are displayed at the top right corner in 00:00 format.

### Win Condition

To win, the player has to cross the finish line before the timer runs out. At the finish line, there is a **collider**. Upon colliding, the player wins.

## States

There are a total of four states in the game. Their flow is as follows: 
```
Start State -> Play State (if win) -> Win State -> Start State 
                          (if lost) -> Lose State -> Start State
```
