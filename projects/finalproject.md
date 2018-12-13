---
layout: post
title:  VR Force Glove
---

Date: December 2018

Category: Mechatronics, Mechanical Design, VR

&nbsp;
&nbsp;

# Virtual reality glove that provides force feedback and simulation


&nbsp;
&nbsp;

## Software

Arduino, C#, Python, Unity

&nbsp;
&nbsp;

# Project description
<img src="./proj/Winter/mario.png" width="370" style="margin-left:auto; margin-right:auto;display:block;"/>

The goal of this project is to create a  virtual reality simulation utilizing the HTC VIVE  VR system as well as designing a hand held device that provides force feedback to the user when interfacing with the virtual reality simulation. The design of the final version of the glove was inspired by the shape of the steam  locking mechanism of the glove was created by using a passive mechanical system. While the Simulation creates a realistic physics environment for the user to interact with using the Unity game engine.

Below is a more in depth discussion of the project. To see the github repository for this project follow this link. The repo contains instructions on how to build and run the software to use the glove.


Creating the Virtual reality simulation  
&nbsp;

To create the virtual relaity simulation I chose to program in the unity game engine. I chose this engine at the time because it had first party drivers for the Vive SDK as well as a large community and many indepth walthroughs that could be used to aid in learning how to use the system.  Another benifit to using unity was that at the time I was creatig the simulation enviornent  it was the only vr compatable game engine that was open to hobbiests.

My goal in the creating of this initial simulation is to have a very basic simulation that have objects interactrealistically with the user and other objects in teh simulation world. Because we wanted the simulation to look reasonably realisting in terms of te underlying physics of how objects move and react to eachother we could not use the base unity physics engine, due to how easy it is for the hand models mesh to clip through objects or collision detection to never trigger, this breakes the immersion of the simulation. TO fix the clipping issue a package called Newton VR was used. Newton VR is an open source physics engine that overhauls the inbult unity engine to create more realistic physics interactions, the most relavent  feature to this project is that Newtonvr objects cannot clip through eachother.

Once the new physics simulation was inserted a proximity based parenting system was created using a method called raycasting. This system in essence locks a grabbed object to have the same transform as the hand object, when the object is within a certain proximity of both the index and thumb fingers. This gives the illusion that the hand has actually picked up and object.

Interfacing with the glove
&nbsp;
Interfacing between the glove and the simulation is acheived by a relativley straightforward solution. The position of the glove in freespace is given using the VIvie puck, and the proprietary trackign algorithems provided in the vive SDK. The degree angles for the actuation of the index finger and the bit that is toggled to determine if the glove should be locked or not is sent via serial communications using an off the shelf bluetooth module.  

Unity only accepts inputs from eithea mouse and keyboard or from a steam compatable gamepad. Since the glove needs to send numerical data as a stopgap measure after data is received from the serial communications line its converted into joystick data using an xbox virtual controller. This controller is able to connect to unity through the steam vr interface, the downside to using a virtual controller is that data pertaining to wheither the glove should be locked or unlocked cannot be sent back through the virtual controller through methods such a rumble feature or changing the values for the lights on a controller. FOr the available open source vitrual controller a response value is unavailable.  to get arround this issue the windows API can be accesseed through unity to take controll of the keyboard and toggle function keys like capslock, numlock, or scrolllock.  With these keys toggalable from unity their current state can be used to determine when an object is beign grasped or not and signal when to send data back to the arduino to change the state of the motor.  

mechatronics in the glove
inorder for hte glove to communicate with the simulaton enviornent it needed a few basic features.
	1. a way to determine what angle the finger is being held at
	2. a locking mechanism
	3. a way to lock/unlock the locking mechanism
	4. communication method with the Vive computer
	5.  a way to determine if the user is trying to apply force to a grabbed object or trying to let go of one

	- locking mehcnism
		- ratchet
		- it was decided to use a passive locking mechanism
			- less battery intensive
			- dosnet require as large or oa powerfull of a motor
			- can be acheived with pretty cheap parts
	- force sensor / strain gague
		- need to bea beable to detect when the user wishes to disengague with the grabbed object
		- used with unty events eg force bar
	- design decisions for things like the controll lever used for locking and unlocking the glove
	- potentiomiter
		- tried to use an imu but there were issues with drifting values and inconsistancy in the oututted data


Designing the glove
&nbsp;

The glove went through two rounds of design the first design of the glove was a very basic model for a proof of design. It was not focuses on ergenomics or ease of useability adn was mostly focused on proving that the use of a ratchet as the locking mechinism was a viable solution. THe major flaws in this design was first and formostly how not straigntforward it was to put on, sesondly the force sensor used was quite flimsy and hand a high possibility of breaking orslipping out of position, the index and thumb cups on the end of each of the protrusions were unconfortable to use and akward to hold when trying to move your index finger and the cups on the end were quite painful in the way that they dung into your skn. And finally the vivie puck was not very securley attacked to the hand which could cause the orientation and angle inside the simulation to be incorrect when the device is active.

THE glove went through a cosmetic redesign to fix the problems found in the first design. Cheifly of which was the ergenomics. IN the redesign the new shape was based off of hte general looks of the steam knuckles or the oculous hand controllers int hat its a sweeping design that is secured by the last three fingers. The mechanical components have remained the same form the first design to the final one. The old force sensor and index finger protrusion has been completely replaced by the addition of a strain gaguue with a liniar slide mounted ontop. in the new design the ratchet head is spun by pushing and pulling on a ring that slides up and down a liniar slide attahed to the strain gague. this mode of actuation is ess akward to use thaan the original finger cuff method and allows for different finger types to use the glove more easily.

- drawbacks of the design choices
&nbsp;
	- the way it acts under load
		- must manually unlock the wratchet when the lever is, will not unload by itself
	- there are very obvious weak points in the glove (controll lever) which could be bent or broken pretty easily
	- because the control lever is needed for locking and unlocking the gloe it limits how small the glove can be due to the distance needed between the two partsand the need for the two parts to be in line with each other





	THe next task was programming the ability to pickup objects. Originally

		- because we wanted to simulation to look as real as possible we couldent use the inbuild unity physics engine because of the inacuracies observed in collisions between the hand bject colliders and the pickup object collider where often times the hand object would pass through the blocks collider causing clipping of the object skins.
		- to prevent these cliping issues we used the package newton vr an open source physics engine for the unity platform. This package is an overhaul of the unity physics engine to create a model that is more similar to reality.
		- with the addition of this package collisions between objects became tighter and more reliable. WIth the reliability of collisions increasingthis presented a problem when tryng to grasp an object between the hand's colliders. the object would repeatidly bounch off of the thumb and forfinger colliders until it would eventually bounch out of the grasp
			- the fix to this is by breaking each of the finger collders into multiple sections and addign raycasts to each of the segments
				- this allows the fingers to be able to grasp an object by parenting the block to the hand object if the the raycasts for the index and thumb fingers are triggered while still preserving the utilty of the newton vr package if an object is not being grabbed
	- this method has its own problems (if an object is small it could completely miss a raycast etc
	- finally because the only method in which unity accepts inputs is through wither the keyboard/ mouse or a gamepad controller. To actuatethe fingers we use the input of a controllers y- axis to determine the positioning of the index fingers, inorder to signal that an object has been grabbed in the unity enviornment on a successful grab the unty scripts will toggle the capslock key. (on signifies that an oject is being grasped, off means that there is no grasped object)

&nbsp;

# Expansion of this project

make a cool simulation
modify the design slightly  to better hide the wires inside the device and andy battery pack the device would need
- add a battery
- expand the design to support multiple fingers

Link to the projects github repository

[source](https://github.com/Laurenhut/Super-Mario-AI)


# References

newton vr
arduino packages
virtual controller packages


&nbsp;
&nbsp;
