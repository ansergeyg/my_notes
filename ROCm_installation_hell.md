I spent a couple of days trying to configure ROCm in VirtualBox ubuntu 20.04 just to find out that the whole idea of doing that was kind of stupid.
Why? Well, simply because when you want to use ROCm you kind of expect your AMD & Radeon(cpu & gpu) to be working in VirtualBox ubuntu image but fundamentally it's impossible due to the nature of the VirtualBox iteself.

There are two major types of hypervisors.

1) Type 1 hypervisor. For example KVM. Kind of (bare metal), acts like a lightweight operating system and runs directly on the host’s hardware.

2) Type 2 hypervisor. For example VirtualBox. Runs as a software layer on an operating system, like other computer programs.

When I was installing all these ROCm related packages in VirtualBox ubuntu, I thought that my virtual hardware is identical to my laptop. Boy, I was wrong. Even though I manually configured vbox images many times, it didn't occur to me
that what was created as virtual hardware was not what I expected at all. It's just not the feature of type 2 hypervisors to allow host to cofigure its virtual hardware. So it comes with predefined virtual hardware.

Ok, but then, may be we need to use type 1 hypervisor? The most popular one is KVM. The only problem is that it works only in linxux OS. My laptop is windows 10. And frankly, even if I find type 1 hypervisor that works in windows 10 it's still not guaranteed
my hardware would be fully supported by that virtual machine or ROCm!

So the real question: Is my hardware fully supported by ROCm?

One last option is to follow amd ROCm installation guide for Windows OS and hope ROCm may support my hardware as well.

UPDATE:

It was very hard to find the exact characteristics of my integrated GPU.

But this source helped:

Even thought (not entirely reliable but checks out if you look up on the internet).

https://www.quora.com/Is-Vega-8-and-Vega-Mobile-Gfx-2-10-GHz-the-same-I-have-laptop-and-it-said-it-had-a-Radeon-Vega-8-but-when-I-see-the-PC-components-the-graphics-card-appeares-as-Vega-Mobile-Gfx-2-10-GHz-Are-these-the-same-thing

Characteristics: Processor AMD Ryzen 5 3500U with Radeon Vega Mobile Gfx, 2100 Mhz, 4 Core(s), 8 Logical Processor(s) 

Radeon Vega Mobile Gfx - is actually Vega 8 or GCN 5.

Checking with official amd gpu support documentation:

https://rocm.docs.amd.com/en/latest/release/gpu_os_support.html

the last supported gpu architecture is GCN 5.1

Goddamn it. Will try to use google colab for now.
