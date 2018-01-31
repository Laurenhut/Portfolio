---
layout: post
title:  Magic Eraser
---
<!-- ![RPS](/img/callout.jpg){: .img-center} -->
<!--
Date: November 2017

Class: Computer Vision

Category: Computer Vision, Python

Partner: William Spies


&nbsp;
&nbsp;

# video post-processing using computer vision.<


&nbsp;
&nbsp;


# Project description


This project's goal is to erase red text written on a piece of newspaper from a prerecorded video.
In the video there is a pen that scans across the newspaper to indicate how much of the text should be erased in that frame.
This project utilizes the color masking to separate what portion of each of the video frames is a part of the newspaper, the pen, or the red text.
The program then utilizes the texture synthesis by non-parametric sampling algorithm to replace the red text with a pattern that matches the surrounding newspaper.



&nbsp;
&nbsp;

Below is a link to repository with the final code.

[Github Repository](https://github.com/Laurenhut/magic-eraser) -->

Date: May 2017

Class: Senior Design

Category: Control systems, mechatronics, C, Python

Partners: Spencer Nelson, Silas Kuchta, Loagan Baerenwald


&nbsp;
&nbsp;

# A new way to tackle remote-person view


&nbsp;
&nbsp;

## Hardware
HTC Vive, Raspberry Pi, Tiva 123-g development board


&nbsp;
&nbsp;

# Project description
<!-- ![RPS](/img/headset.jpg)
<!-- {: .img-center} -->

This project address the need for there to be a more engaging first person video experience for hobbyists
 that can utilize commercially available VR headsets. The prototype uses an HTC Vive VR headset connected
 to a Walkera gimbal through a Raspberry Pi Zero and the ARM cortex -M4F microcontroller on the Tiva C 123g
 development board. An API running on the computer connected to the HTC Vive collects the data output of the
 headset, and sends it through WiFi to the prototype. At the same time, real time video is captured with a compact
 camera attached to the Raspberry Pi, and sent back to the computer through a simple web interface.
