# Is ros_control realtime?
## Question (link)[https://answers.ros.org/question/191288/ros_control-and-rt/]
> Dear all,
> 
> The doc for ros_control says controller_manager provides a hard RT loop.
> 
> knowing that ROS is designed for non-RT OS (plain linux), how is that possible on a non-RT OS?
> how does that relate to package realtime_tools ?
> Thanks,

## Answer 1:
>> The doc for ros_control says controller_manager provides a hard RT loop.

>> knowing that ROS is designed for non-RT OS (plain linux), how is that possible on a non-RT OS?

> It isn't. Not without first making Linux capable of some deterministic scheduling and execution.
> 
> Two well known ways to add deterministic scheduling to a Linux system are to patch the kernel with the RT PREEMPT patch, or to install the Xenomai co-kernel next to it. The PR2 (which runs the pr2_mechanism package, the precursor to ros_control) has an RT PREEMPT patched kernel.
> 
> Whatever system is used, 'real-time-ness' can be configured with a per-thread granularity. So even if 'the rest' of ROS is not inherently real-time, parts of nodes can be made to be deterministically scheduled and executed. In the case of ros_control, the main loop responsible for executing all the instantiated controllers is an example of such a real-time thread. As such, the controller plugins themselves execute in a real-time context.
> 
> The fact that ROS itself (which is an ambiguous thing to say anyway) is not real-time does not really matter.
> 
> The only thing you need to take care of now is to make sure that you can safely communicate with the real-time parts of your node.

>> how does that relate to package realtime_tools ?

> One of the main issues when programming on / within real-time (sub)systems is breaking the determinism achieved by the scheduler. To avoid this -- without a priori checked execution times and things like time-slicing and round-robin -- every process under the real-time scheduler basically has to make sure it doesn't hog the cpu for too long (ie: it should yield often enough). Any service (not a ROS service, but a syscall fi) that a process depends on which cannot be interrupted could potentially make this impossible. Memory allocation is one such service: if an allocation request results in a page fault, all bets are off wrt how long it takes the OS and the hardware to handle it. Locking of resources between non- and real-time threads (mutexes, semaphores) is also something that needs to be done with care, as priority inversions might cause undesirable scheduling latencies.
> 
> The realtime_tools package provides implementations of the ROS publish and subscribe mechanism that take these limitations into account. Memory allocation in the real-time components is done in the initialisation phase, and all dynamic message handling is done by non-real-time helper threads that would not cause any problems if they did require non-real-time safe services.
> 
> Some related questions which I did not see in the Related questions box:

# How to use RT kernel with ROS:
## Question (link)[https://answers.ros.org/question/61162/set-up-for-real-time-control-in-ros/]
> Hello,
> I am new to real-time control in ROS. When I looked up it in this wiki, integrating ROS and OROCOS and using Xenomai seems to be a good solution for it. I don't have a high level understanding of the advantage of using them. If you could share your insight into it with me, I would really appreciate it.
> 
> I can enumerate several possible set-ups for real-time control in ROS although some of these may not make sense.
> 
> 1.ROS on Ubuntu
> 2.ROS on Xenomai patched Ubuntu
> 3.ROS+OROCOS on Ubuntu
> 4.ROS+OROCOS on Xenomai patched Ubuntu
> We may scarcely be able to expect any good real-time performance on (1). How/why bad is it? Would be there any improvement in using (2) over (1)? How would (3) be different to (4) in real-time performance? Do you have any other recommended set-up? Dose it need to be broken down into some subdivided set-ups?
> 
> Thank you very much in advance.
## Answer 1:
> You can only get good realtime performance with option 4. The OROCOS RTT toolkit provides realtime behaviour but only on a kernel with a realtime extension. The other options can also give good performance but it will vary a lot. See also this: http://answers.ros.org/question/9500/what-realtime-patch-should-i-use-with-ros/
> 
> A good example is how the realtime controllers of the PR2 work. ROS runs non realtime while the controller manager (which uses the RRT toolkit I think) is realtime.
> 
> image description From:http://pr2support.willowgarage.com/wiki/PR2%20Manual/Chapter7#Realtime_Loop

## Answer 2:
> In our research group we have used OROCOS for several years, and have hired some of the OROCOS developers as consultants to help us tune the real-time performance of our system. I would recommend starting with the easiest to set up system (ROS on a vanilla kernel, components written in Python), and make the following upgrades ONLY as necessary after MEASURING your current performance, and ONLY to the parts of your system that NEED it:
> 
> Make sure you have your components/nodes that need it running with real-time priority
> Upgrade your kernel to one with the PREEMPT_RT patches applied
> Dedicate a fraction of the cores on your CPU (if multicore) to your real-time processes, make sure the non-critical processes are launched on the other cores.
> Port components to C++ if delays within the components are the bottleneck
> Port components to Orocos if delays of message transmission between components is the bottleneck
> Install Xenomai kernel and recompile against it's API's (at this point you are pretty deep into black magic territory; the only people using this are either operating obscenely tight control loops, or implementing safety-critical systems)
> Never make any assumptions about where your bottleneck is; ALWAYS measure at every stage of development. Rules of thumb like "avoid dynamic memory allocation" and "only use low-level systems programming languages like C/C++/Fortran" come secondary to this.


# Install xenomai kernel:
## Blog
(link)[https://www.ashwinnarayan.com/post/xenomai-realtime-programming/]