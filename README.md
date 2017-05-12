# CarND-Controls-PID
Self-Driving Car Engineer Nanodegree Program

---

## Reflection

### PID Components

The Proportional, Integral, and Derivative (PID) components are described below.  They all operated as I expected.

#### Proportional Component

The proportional component is primarily responsible for steering the car toward the center-line, proportionally to how far the car is away from the center line.  As the car gets further from the center line, the car will steer harder back toward center.  When the proportional component is used alone, the car will overshoot because it won't start countersteering until it has passed the center line.

#### Integral Component

The integral component is proportional to the accumulated error.  As error accumulates, this term will have a greater impact, effectively adding a bias to correct any persistent error.  For example, if the car were to pull slightly to the right, error would accumulate that when multiplied by the integral gain would bias the car to steer slightly to the left to compensate.

#### Derivative Component

The derivative component is proportional to the difference in cross-track error between time steps.  This component allows the car to counter-steer, helping to prevent overshooting.  As the car approaches the center line, the proportional control will continue to steer toward the center.  The derivative component will steer away from the center line, more agressively as the car moves more quickly toward the center line.  If the car is moving very quickly toward the center line, this component can overwhelm the proportional component, so that the car matches the center line instead of overshooting.  

### Hyperparameter Selection

I used the [Ziegler–Nichols](https://en.wikipedia.org/wiki/PID_controller#Ziegler.E2.80.93Nichols_method) method to tune the PID controller.  The Ziegler-Nichols method involves gradually increasing the proportional gain, with the integral and derivative gains set to zero, until the car begins to oscillate.  This is the point of ultimate gain, K_u.  T_u is the period of oscillation at this gain (i.e. how many messages are received per cycle).  These values can then be used to calculate optimal gains for the PID components.

The car started oscillating with proportional gain set to K_u=0.2, with a cycle of T_u=65 messages in my tests.  With that data, I was able to calculate values for each of the PID gains, where K_p = 0.6 * K_u, K_i = 1.2 * K_u / T_u, and K_d = 3 * K_u * T_u / 40.

## Dependencies

* cmake >= 3.5
 * All OSes: [click here for installation instructions](https://cmake.org/install/)
* make >= 4.1
  * Linux: make is installed by default on most Linux distros
  * Mac: [install Xcode command line tools to get make](https://developer.apple.com/xcode/features/)
  * Windows: [Click here for installation instructions](http://gnuwin32.sourceforge.net/packages/make.htm)
* gcc/g++ >= 5.4
  * Linux: gcc / g++ is installed by default on most Linux distros
  * Mac: same deal as make - [install Xcode command line tools]((https://developer.apple.com/xcode/features/)
  * Windows: recommend using [MinGW](http://www.mingw.org/)
* [uWebSockets](https://github.com/uWebSockets/uWebSockets) == 0.13, but the master branch will probably work just fine
  * Follow the instructions in the [uWebSockets README](https://github.com/uWebSockets/uWebSockets/blob/master/README.md) to get setup for your platform. You can download the zip of the appropriate version from the [releases page](https://github.com/uWebSockets/uWebSockets/releases). Here's a link to the [v0.13 zip](https://github.com/uWebSockets/uWebSockets/archive/v0.13.0.zip).
  * If you run OSX and have homebrew installed you can just run the ./install-mac.sh script to install this
* Simulator. You can download these from the [project intro page](https://classroom.udacity.com/nanodegrees/nd013/parts/40f38239-66b6-46ec-ae68-03afd8a601c8/modules/aca605f8-8219-465d-9c5d-ca72c699561d/lessons/e8235395-22dd-4b87-88e0-d108c5e5bbf4/concepts/6a4d8d42-6a04-4aa6-b284-1697c0fd6562) in the classroom.

## Basic Build Instructions

1. Clone this repo.
2. Make a build directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./pid`. 

## Editor Settings

We've purposefully kept editor configuration files out of this repo in order to
keep it as simple and environment agnostic as possible. However, we recommend
using the following settings:

* indent using spaces
* set tab width to 2 spaces (keeps the matrices in source code aligned)

## Code Style

Please (do your best to) stick to [Google's C++ style guide](https://google.github.io/styleguide/cppguide.html).

## Project Instructions and Rubric

Note: regardless of the changes you make, your project must be buildable using
cmake and make!

More information is only accessible by people who are already enrolled in Term 2
of CarND. If you are enrolled, see [the project page](https://classroom.udacity.com/nanodegrees/nd013/parts/40f38239-66b6-46ec-ae68-03afd8a601c8/modules/f1820894-8322-4bb3-81aa-b26b3c6dcbaf/lessons/e8235395-22dd-4b87-88e0-d108c5e5bbf4/concepts/6a4d8d42-6a04-4aa6-b284-1697c0fd6562)
for instructions and the project rubric.

## Hints!

* You don't have to follow this directory structure, but if you do, your work
  will span all of the .cpp files here. Keep an eye out for TODOs.

## Call for IDE Profiles Pull Requests

Help your fellow students!

We decided to create Makefiles with cmake to keep this project as platform
agnostic as possible. Similarly, we omitted IDE profiles in order to we ensure
that students don't feel pressured to use one IDE or another.

However! I'd love to help people get up and running with their IDEs of choice.
If you've created a profile for an IDE that you think other students would
appreciate, we'd love to have you add the requisite profile files and
instructions to ide_profiles/. For example if you wanted to add a VS Code
profile, you'd add:

* /ide_profiles/vscode/.vscode
* /ide_profiles/vscode/README.md

The README should explain what the profile does, how to take advantage of it,
and how to install it.

Frankly, I've never been involved in a project with multiple IDE profiles
before. I believe the best way to handle this would be to keep them out of the
repo root to avoid clutter. My expectation is that most profiles will include
instructions to copy files to a new location to get picked up by the IDE, but
that's just a guess.

One last note here: regardless of the IDE used, every submitted project must
still be compilable with cmake and make./
