## Title: Automotive Grade Linux (AGL) - meta-ros

## Summary:

The integration of the Robot Operating System (ROS) Framework as an option within Automotive Grade Linux (AGL) through the meta-ros yocto layer is an important step in upgrading vehicle technology using automotive systems. This project is important for several reasons:
- Bridging the Gap Between Robotics and Automotive Software.
- Expanding AGL’s Capabilities
- Advancing Open-Source Development
- Enhancing Real-World Use Cases

## Objectives:
- To integrate meta-ros scarthgap layer onto AGL master branch for further releases.
- Bring-up the same and get it working on Pi500.
- Write a demo application showing interaction and communication between ROS libraries and AGL Application.

## Journey:
It involved a lot of aspects, once we hear about integrating the Yocto layer, like meta-ros. Initially after the communication period, I thought of adding the bitbake-layer and building a default target for AGL, so I cloned meta-ros and added that as a bitbake layer into my build environment. I succeeded in doing so and was able to compile ros-core after adding the layer.

Next after this came the hardware to test this on, initially I was testing it on QEMU x86_64, but our goal was to run this on actual hardware, so the next task was to run this on hardware itself. At this time, I tried building raspberrypi5 target, which did boot-up but was glitchy on the frontend and the UX, afterwards we found out (thanks to my mentors), that kernel source being included was quite old, and it had to be upstreamed or include necessary patchsets to be able to boot on pi500, a patch was sent out to add dts support on the kernel builds, which did fix our issue.

Afterwards, since till now the major layer add-on was manual, AGL actually works on the basis of feature that we include before starting an actual build, these are known as AGL features established by AGL feature templates, present in layers, since ROS is a development feature, it had to be added inside meta-agl-devel, I sent out the patchset to add the AGL feature called as meta-agl-ros2 inside agl-devel, it’s major role was to now include ROS 2 layers to be able to build ROS packages, like ros-core, etc.

Now, the role was to check subscribers and publishers on the hardware, so after booting up the hardware we were able to send and receive messages via ROS, using ROS specific tutorials, which were now working in AGL!
Next, was to start on the demo application, What is the scope? What can we do? Frontend? Backend? Etc. The reason for having an application was that, majorly so far we have seen terminal based tutorials and demos in a lot of places, and having an actual UI/UX to showcase a feature was a major amount of work in-addition to having the only libraries working. So, we brainstormed regarding the scope of the application.

One of the initial features of the application included, having the ability to establish communication with ROS libraries, for this there are 2 flutter libraries, ros-bridge and rcldart. We went with rcldart, since it is a recent and under development library with respect to ros-bridge, we had a conversation with the maintainer of rcldart for permission to use his library, which we eventually was able to!

Now, the communication between ROS and flutter applications was working on my system, and when we tested it on the hardware, we were not able to, it was a user namespace issue, AGL applications run on a different user, but we were able to resolve this, and test custom subscribers and publishers.

Now, we had to come up with a real world usecase, where we now decided to use a camera! Which made us brainstorm about what we can do, I tried out simulators to simulate cars and roads, and test out ROS, but that gave us not much of a conclusion, afterwards, we came up with using object detection mechanisms, such that while driving cars can detect objects real-time and press brakes and alert the driver as well, I implemented that in the application, and now it was working, since we had time, next up was doing something else with the camera, so we added a mood detection scenario for the driver, such that if a driver falls asleep, feels fatigue, based on camera readings on the dashboard, the car is able to alert the user and press brakes accordingly.

## Results:
- Was successfully able to integrate meta-ros scarthgap on AGL master.
- Pi500 now boots successfully with raspberrypi5 target.
- The demo application is now extended from just subscriber/publisher sync, it also has camera detection running a model in background in ROS-end to be able to do face detection and person detection.

## Challenges:
- Initially Pi kernel upstream was quite old, which lacked dts support, so we had to add that in order to be able to boot-up Pi500.
- The ROS backend, which ran on the terminal, was not able to communicate with the application, we were able to debug this by finding out the userspace/namespace issues where the backend was running.
- A lot of glitches and components in the application, which got fixed along the way.

## Future Scope:
- Upstream the meta-ros layer and in-sync with further releases of AGL.
- Test the upstreamed layer with base use cases along with application.
- Maintain the application for higher upstream and for further hardware updates.
- Establish gRPC communication with AGL frontend, to have an end-to-end communication setup between AGL and ROS.

## Patch-sets:
- https://gerrit.automotivelinux.org/gerrit/c/AGL/meta-agl-devel/+/31201?usp=dashboard
- https://gerrit.automotivelinux.org/gerrit/c/AGL/meta-agl-devel/+/31175?usp=dashboard
- https://gerrit.automotivelinux.org/gerrit/c/AGL/meta-agl-devel/+/31265?usp=dashboard
- https://gerrit.automotivelinux.org/gerrit/c/AGL/meta-agl-devel/+/31266?usp=dashboard
- https://gerrit.automotivelinux.org/gerrit/c/AGL/AGL-repo/+/31178?usp=search
- https://gerrit.automotivelinux.org/gerrit/c/AGL/meta-agl-devel/+/31100?usp=search
- https://github.com/danascape/ros-flutter

## Images:

