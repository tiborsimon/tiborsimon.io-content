title: SlotCar Race
tags: embedded, dsp, accelerometer
repo_link: https://github.com/tiborsimon/slotcar-race
version: v1.0
Date: 2015-01-15
Summary: At the university me and my partner have faced a midterm project that contained a very interesting problem. We had to write an algorithm that drives a small electric car on it’s guided track.
Repo: https://github.com/tiborsimon/slotcar-race
Release_link: https://github.com/tiborsimon/slotcar-race/releases/latest


The only information we had was the acceleration data comes from the car itself. Based on this information we had to control the voltage of the car’s motor to run the fastest lap on the track. It doesn’t sounds too hard, does it.

<div class="gallery">
    <img class="jslghtbx-thmb" src="/images/articles/slotcar/promo1.jpg" alt="Small slot car that has to be driven autonomously." data-jslghtbx data-jslghtbx-caption="The slot car we have to program." data-jslghtbx-group="slotcar-group-1" />
    <img class="jslghtbx-thmb" src="/images/articles/slotcar/promo2.jpg" alt="The track the slot car has to run." data-jslghtbx data-jslghtbx-caption="The track the slot car has to run." data-jslghtbx-group="slotcar-group-1" />
</div>

# About the competition

However. The acceleration data was very noisy, it needed a strong filtering, therefore it had serious amount of delay. We had to cope with this delay and we had to run the fastest lap among the other teams who participated this challenge without sliding off the track.

The rules of the competition

- Run the fastest 5 lap without sliding off the track.
- No physical modification were allowed.
- You have 3 attempts.

# The track

The track was a casual slot car track with two corners and a cross over section. The power for the car comes from the two pair of rails. There is a room for two cars on the track at once.

<figure>
    <img src="/images/articles/slotcar/track.jpg" width="400" alt="The track.">
</figure>

# The car

Audi R8 body with an embedded Freescale processor in it. In between the front wheels there is a guider with the brushes to collect the electricity from the rails. There are two IO interface on the car: the mini USB connector for programming and a mini SD card slot to get the accelerometer data for developing. One car had a led on the top of it that we could use for debugging the algorithm.

<figure>
    <img src="/images/articles/slotcar/car.jpg" width="400" alt="The slot car.">
</figure>

# Solving the problem

There is a trivial solution for the problem: drive fast in the straight parts, brake before the corners, drive slow in the corners.
But there are a few questions.

- What is the maximum speed the car can keep without sliding off the track.
- How do you know where are you on the track?
- How do you compensate the delay of the digital filter.

The first question was a simple one. We slowly increased the motor voltage and run a few laps until the car slid off. But there was a small problem. Not all cars behaved exactly the same. Some cars slid off at the given motor voltage value, some cars didn’t. We had to take account of this behavior.
The second question was a tougher one. The car only knew the acceleration data measured by itself. These data could be saved on a memory card during the runs, so we could analyse them offline.

You can see the data coming from the car when it run 4 loops on the track on the picture down below. As you can see the only usable information comes from the X acceleration data. After the car speeds up, there is a nice periodic signal. Our main task was: find the period of the signal, so we could figure out where we are on the track. When we have the information about where we are, we can create an algorithm that drives the car faster on the straight lines, and slower in the corners. The period finder algorithm had to be prepared to find the period regardless of the starting position of the car.

The third question was easier after the second one. We knew that the acceleration data had a delay in it. We had the knowledge about where we are on the track with a delay. So instead we make our decisions about braking or accelerating after we reached a straight line or a corner, we make a prediction before.

Let’s see how the car performs with the final algorithm before we dive into the details.

<iframe class="video" width="640" height="360" src="https://www.youtube.com/embed/Aj9IzfBdFBo" frameborder="0" allowfullscreen></iframe>

# What is happening on the video?

You will learn later from the source code listed below, that the whole program stands from three main states:

- Normalization state – we are waiting the car to speed up to the maximum safe speed, and to run in a straight section of the road.
- Period finder state – the car runs at least two lap before it has enough information to run the period finder algorithm.
- Final state – the car accelerate and deccelerate according the information gathered in state 2.

Let’s see how this happened on the video.

The car was put down on the track. It accelerated to the maximum safe speed. It reached this speed a bit before the first corner, so it was on a straight section when the acceleration finished, therefore it entered state two, which is the period finder algorithm. The small blue led on the top of the car flashed when the car’s state changed, but the led flashed in the middle of the corner because the delay of the acceleration filtering.
In state two the car run at least two full lap (because it collects at least two of every kind of event), collected informations, and after the second lap, when there was enough information, the period finder algorithm kicked in, and successfully found the period of the track.
Right at this moment, the car was switched to state three, in which the algorithm takes care of the car’s speed according to the information collected in state two. The led blinked in every lap.

# Period finder algorithm

<figure>
    <img src="/images/articles/slotcar/raw-data.png" width="550" alt="Raw accelerometer data">
    <figcaption>Raw accelerometer data</figcaption>
</figure>

The period finder algorithm is a very simple thus a very specific algorithm that was developed only for this competition. It may work in other situations, but don’t bet your money there :)

<figure>
    <img src="/images/articles/slotcar/algorithm.png" width="550" alt="lgorithm output signals">
    <figcaption>Algorithm output signals</figcaption>
</figure>

As you can see, the final output of the algorithm was shown above.

Below you can find the repository for the nicely commented source code of the period finder algorithm. The final code that runs on the car itself is not showed, it is up to you to implement it in the target platform.

# Summary

With this algoritm you will be able to have a strong foundtation for your own algorithm if you are faceing a similar contest. Feel free to contribute to the algorithm, to make it more durable.
