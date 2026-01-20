# 0. Who Am I

## Slide

- FPGA Engineer (5 years)
- Cosylab, Aviat
- Tired of rewriting basic HDL blocks
- Adopted Open Logic Library
- Open Logic contributor

## Narration

My name is Rene Brglez. I have been working in the FPGA industry for about five years, so I still consider myself relatively new to the field.
I spent four years at Cosylab, and for a little over a year now I’ve been working at Aviat.

At Cosylab, we had a solid in-house HDL library. However, when I changed jobs, I no longer had access to it. The HDL library at Aviat was usable but unmaintained, with almost every component added by a different person and following a completely different coding standard.
That experience made me wonder:
Why do so many companies keep reinventing the wheel and rewriting the same basic HDL building blocks?

This question led me to look for an open-source HDL library. I found several, but the one that impressed me the most was the Open Logic Library.

I started using Open Logic and promoting it internally at Aviat. Today, it is used in one official project and several proof-of-concept projects. Along the way, I also became a minor contributor.

# 1. What Is Open-Logic

## Slide

- HDL Library
- Provides reusable basic building blocks (FIFOs, CDCs, UART, SPI, I2c, AXI4 endpoints, ...)
	- CDCs, FIFOs, Width Converters, Arbiters, CRC, ..
	- UART, SPI, I2C, AXI, ...
- Its not a collection of full IP cores (Designed to support and accelerate your own IP development)
## Narration

Open-Logic is an HDL library that provides a set of reusable, well-tested building blocks, including FIFOs, clock-domain crossings, arbiters, and many more.

The goal is to avoid rewriting the same low-level infrastructure in every project. Open Logic does not try to solve your system for you. Instead, it provides reliable foundations that let you focus on developing your own IP and system-specific logic.

Unlike OpenCores, which offers complete, ready-to-use IP cores, Open-Logic provides only basic, reusable components that designers can use to build their own IP cores.

# 2. What is Open-Logic 

## Slide

- built from a users perspective
	- focus on thorough documentation
	- metrics for code quality
	- tutorials
	- simplicity / ease of use
## Narration

It’s built entirely from the user’s perspective, with a strong emphasis on thorough documentation. Each component also includes live code quality metrics showing statement coverage, branch coverage, and any known issues, so users can easily track the quality and reliability of the component over time and see if any component’s quality degrades or improves.

It comes with tutorials. So you can easily follow them to integrate Open-Logic in any of the supported toolchains.

There is a strong emphasis on simplicity and ease of use. The best way to illustrate this is with an example. 

If we look at the implementation of olo_base_fifo_sync, it may at first seem daunting due to the large number of generics and ports. However, a closer inspection shows that many of these generics and ports have default values and can be omitted if they are not relevant. For example, the FIFO instantiation shown below uses only two generics. Apart from the clock and reset, it exposes just four additional ports, two for the input path and two for the output path.


# 3. Philosophy

## Slide

- pure HDL
	- any device (AMD, Altera, Microchip, Efinix, Gowin)
	- written in VHDL
	- Commitment for Usability from Verilog
- Trustable Code
	- proper CI setup (Simulation, Linting, Synthesis)
	- 100% Statement/Branch Coverage
	- Issue Indicator
- FreeTooling
	- Enabler for Community
## Narration

- pure HDL

Open Logic components are written in pure HDL. This means that it can be used on any device from any vendor. Currently, Open Logic explicitly supports AMD, Altera, Microchip, Efinix, Gowin, and CologneChip.

It likely works on other vendors as well since it is written in pure HDL. However, for Open Logic to officially support a vendor, we must validate it using that vendor’s supported tools and development board. Because these tools and boards are not free, unless the vendor provides a development board and tool licenses or we purchase them ourselves, we cannot perform the required testing. Without this validation, we cannot explicitly claim that Open Logic supports that vendor.

Open Logic is written in VHDL with a clear commitment to be usable from Verilog. All Open Logic components expose only generics and ports whose types are compatible with Verilog, allowing them to be instantiated through Verilog wrapper code in mixed-language toolchains.

- Trustable Code

To ensure that the components in the Open Logic Library are trustworthy, a robust Continuous Integration (CI) setup is in place. This CI pipeline verifies all components through simulation, performs linting to enforce the prescribed coding standards, and synthesizes the designs using all tools officially supported by Open Logic.

The documentation includes badges for each component that display the current statement and branch coverage as well as any open issues. These badges make it easy to track the verification status and monitor code quality over time, helping identify if a component’s quality degrades.

- FreeTooling

By relying heavily on open-source tools, Open Logic can be used and contributed to without requiring expensive vendor licenses. While commercial tools can still be used if available, they are not mandatory. For simulation, either GHDL or NVC can be used. Testbenches are written using the VUnit framework, which is also open source. For linting, Open Logic uses VSG, another open-source tool. In addition, Open Logic recently added official support for open-source synthesis using Yosys and implementation using nextpnr, which are used for the CologneChip vendor.

# 4. Why use Open Logic?

## Slide

- Don't waste time on developing code that exists
- Don't take risks debugging your own CDC/FIFO/...
- Spend your time on your application ...
	-  ... not on well known Basic Elements
- Why Not? It's free

## Narration

- Don't waste time on developing code that exists
- Don't take risks debugging your own CDC/FIFO/...
- Spend your time on your application ..
	-  .. not on well known Basic Elements

There is little value in spending time developing code that already exists. Doing so also introduces unnecessary risk, particularly when re-implementing complex and error-prone building blocks such as clock domain crossings, FIFOs, and similar infrastructure components.

Your time is better spent developing your application, not reinventing well-known fundamental components. It is unlikely that a custom FIFO will significantly outperform a mature, widely used implementation. By relying on established, documented, and thoroughly tested building blocks, you reduce risk, improve reliability, and accelerate development, allowing you to focus on what truly differentiates your design and where your expertise matters most.

- Why Not? It's free

Why not use Open Logic? It is free, so the barrier to entry is very low. With only a small time investment, you can determine whether it adds value to your workflow.

# 5. Better Quality

## Slide

- Portable/Reusable code
	- Move along with technology
	- choose the best fitting device
- Reward Innovative Vendors
## Narration

Using Open Logic leads to higher-quality, more reusable code. By relying on standardized and well-tested components, your designs become easier to maintain and reuse.

When working on a project, new devices may emerge that better fit your requirements. In such cases, you want the freedom to adopt that technology without rewriting large parts of your code base. This is only possible if your code is portable, ideally not just across devices but also across vendors. That freedom allows you to select the best device for your application, ultimately improving its overall quality.

Open Logic also makes code more reusable. By avoiding hard-coded, vendor-specific macros, the code becomes cleaner and easier to adapt. 

Finally, portable code benefits the entire ecosystem. When developers are not locked into a single vendor, they can choose the most innovative and well-supported devices. This rewards vendors who deliver high-quality products, strong support, and meaningful innovation. It is unfortunate when vendor lock-in prevents us from adopting better solutions. Developers should have the freedom to choose, and that freedom encourages vendors to compete on quality rather than lock-in.

# 6. Lower Cost

## Slide

- Reduced Development Effort
- Reduced Maintenance Effort

## Narration

Adopting Open Logic can significantly reduce development and maintenance effort, leading to lower overall costs, since it eliminates the need to re-implement basic building blocks for devices from different vendors or even for different devices from the same vendor.

Consider proprietary in-house HDL Library. How many engineers have actually reviewed it, and how confident are you that is is free of errors? How often is it tested? Do you even have continuous integration pipelines in place, and if so, how frequently are they run? In many cases, these HDL libraries receive limited scrutiny and limited long-term maintenance.

Open Logic, by contrast, is open source. It is reviewed by a much larger group of engineers and supported by regularly scheduled CI pipelines. When anyone discovers a bug or issue, it is quickly addressed and fixed for the benefit of the entire community. This shared maintenance model significantly reduces the ongoing burden on individual teams.

Infrastructure projects like Open Logic strengthen the entire ecosystem. We should not be competing over who can build the best basic components. Our real value lies in designing and delivering the best applications on top of them.

# 7.  Reduce Risk

## Slide

- Lower Debugging Risk
    - FIFOs, CDCs, ...
- Vendor-Agnostic, Portable FPGA Design
- Mitigates Supply-Chain Disruptions
- Reduces Exposure to Trade Restrictions
- Protection Against Device End-of-Life (EOL)
## Narration

Adopting Open Logic reduces project risk in several important ways. First, it lowers the likelihood that you will need to debug fundamental building blocks such as FIFOs, clock-domain crossings (CDCs), arbiters, and similar components. These components are widely used, well reviewed, and continuously tested. If an error is discovered, raising an issue on GitHub gives access to a broad community of engineers who can help diagnose and resolve the problem. Any fix then improves the building block for everyone, not just for you.

Open Logic also enables vendor-agnostic designs at no extra cost. By avoiding vendor-specific dependencies, your design becomes portable and can be moved more easily between FPGA devices. Recent events such as COVID-related supply-chain disruptions have shown how risky single-device or single-vendor dependence can be. When a targeted FPGA is unavailable, portable code allows you to switch to an alternative device that is actually in stock.

In today’s environment of tariffs and trade restrictions, this portability further reduces exposure to geopolitical risk. For long-lived projects, it also provides protection against device end-of-life. Writing portable code from the start makes it far easier to migrate to newer devices in the future, extending the usable lifetime of the design and reducing long-term risk.

# 8. Content Overview


# 9. Call to action

## Slides

- Give Open Logic a Star on GitHub
    - It helps the project get visibility.
- Contribute in any way you can
	- Report Bugs
	- Fix Typos
	- Improve Documentation
	- Ask for features
	- Contribute a feature
- Support financially (optional)
    - GitHub Sponsors, 
    - priority support, 
	- workshops
### Narration

Alright, if you like Open Logic, the easiest thing you can do is give it a star on GitHub. Yeah, it sounds a bit vain, but it really helps the project get noticed.

And of course, you can contribute, and you don’t have to write code for that. Even small things matter. Reporting a bug, asking for a new feature, fixing typos in the documentation, or pointing out if a figure is wrong or misleading. All of that helps improve the project for everyone.

If you want to contribute code, that is fantastic. One important tip is to always start by raising an issue first. This way you will not work in isolation only to find out that your work is not needed because the feature already exists or can be created using existing building blocks.

I learned this myself. I wanted to implement a FIFO that takes N-bit input and outputs M-bit output. At first it looked like Open Logic did not support that. After opening a GitHub issue, the Maintainer informed me I could simply use two existing building blocks, a standard FIFO and a width converter, and combine them. This gave me exactly the functionality I needed without extra implementation.

And finally, there’s the financial side. Running the project has costs, and the Maintainer and other Contributors invest a lot of time for free. If you want to support it, GitHub Sponsors is a great option. If your company doesn’t allow donations, there’s another way: you can buy priority support or a workshop. That way, you help the project and get something in return.

So, star the project, contribute in any way you can, and if you want, support it financially. Every bit helps and every bit is appreciated.

