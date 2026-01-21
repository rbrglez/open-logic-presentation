# 0.0. Who Am I

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

# 1.0. What Is Open-Logic

## Slide

- HDL Library
- Provides reusable basic building blocks (FIFOs, CDCs, UART, SPI, I2c, AXI4 endpoints, ...)
	- CDCs, FIFOs, Width Converters, Arbiters, CRC, ..
	- UART, SPI, I2C, AXI, ...
- Its not a collection of full IP cores (Designed to support and accelerate your own IP development)
## Narration

Open Logic aims to be for HDL what the Standard Library is for C++

It is an HDL library that provides a set of reusable, well-tested building blocks, including FIFOs, clock-domain crossings, arbiters, and many more.

Unlike OpenCores, which offers complete, ready-to-use IP cores, Open-Logic provides only basic, reusable components that designers can use to build their own IP cores.

# 2.0. Built for Users 

## Slide

- focus on thorough documentation
- metrics for code quality
- tutorials
- simplicity / ease of use
## Narration

It’s built entirely from the user’s perspective, with a strong emphasis on thorough documentation. Each component also includes live code quality metrics showing statement coverage, branch coverage, and any known issues, so users can easily track the quality and reliability of the component over time and see if any component’s quality degrades or improves.

It comes with tutorials. So you can easily follow them to integrate Open-Logic in any of the supported tool-chains.

There is a strong emphasis on simplicity and ease of use. The best way to illustrate this is with an example. 

# 2.0. Built for Users 

## Narration

If we look at the implementation of olo_base_fifo_sync,  on the right, it may at first seem daunting due to the large number of generics and ports. However, a closer inspection shows that many of these generics and ports have default values and can be omitted if they are not relevant. For example, the FIFO instantiation taken from the vivado tutorial of the open-logic, shown on the left uses only two generics. Apart from the clock and reset, it exposes just four additional ports, two for the input interface and two for the output interface.

# 3.0. Language

## Slide

-  Written in VHDL
- Commitment to be usable from Verilog

## Narration

Open Logic is written in VHDL with a clear commitment to be usable from Verilog. All components expose only generics and ports whose types are compatible with Verilog, allowing them to be instantiated through Verilog wrapper code in mixed-language tool-chains.

# 4.0. Key Benefits

## Slide

- Reusable Building Blocks
- Device/Vendor Portable
	- AMD, Altera, Microchip, Efinix, Gowin, CologneChip
	- Choose the best fitting device
- Reduced Risk
- Freedom to Choose
## Narration

Using Open Logic leads to higher-quality, more reusable code. By relying on standardized, well-tested components, designs become easier to maintain and reuse.

Because it is written in pure HDL, Open Logic is inherently portable and should run on any toolchain, though regression testing currently covers only the officially supported vendors. By avoiding vendor-specific macros, designs can be moved easily between FPGA devices.

This portability provides resilience against evolving requirements, supply-chain disruptions, trade restrictions, tariffs, and device end-of-life. Projects can switch to available alternatives, simplify migration to newer devices, extend the usable lifetime of designs, and reduce long-term maintenance risk.

Beyond individual projects, portability benefits the broader FPGA ecosystem. When developers are free to choose their tools, vendors must compete on quality, support, and innovation rather than lock-in, ultimately leading to better devices, better tools, and better outcomes for everyone.

# 5.0. Trustable Code

## Slide

- proper CI setup (Simulation, Linting, Synthesis)
- 100% Statement/Branch Coverage
- Issue Indicator
## Narration

To ensure that the components in the Open Logic Library are trustworthy, a robust Continuous Integration (CI) setup is in place. This CI pipeline verifies all components pass simulation, performs linting to enforce the prescribed coding standards, and synthesizes the components using all tools officially supported by Open Logic.

# 5.1. Trustable Code

## Slide

## Narration

To illustrate the importance of this CI setup, I’ll use an example.

Open Logic provides a generic CRC component, but for my use case, I needed an additional feature. Originally, this component only supported calculating the CRC over the entire data bus. In my example, I was working with a 16-bit data bus. When sending a packet, the last data beat, that is, a single AXI-Stream data transfer in one clock cycle, could contain either two bytes (16 bits) or only one byte (8 bits). If the final beat contained just a single byte, the original component could not calculate CRC for the packet.

To address this, I submitted a pull request adding a Byte Enable (`In_Be`) port. This change was fully backward compatible: if the feature was not needed, the port could simply be ignored. I also created a new testbench specifically for this feature, ran the tests, and verified that the CI pipeline for linting and simulation passed without issues.

Thanks to the CI pipeline for synthesis, I discovered that I had added HDL code that was not synthesizable. An excerpt of the un-synthesizable for loop is shown on the right, and below it is the corrected version of the loop that can be synthesized. The pipeline allowed me to quickly identify and fix the issue, preventing a broken component from being added to Open Logic. This example clearly shows how CI not only ensures functional correctness but also guards against introducing unsynthesizable code.

# 5.2. Trustable Code

## Slide

## Narration

After my pull request adding a byte-enable port was merged into Open Logic, the maintainer added support for another vendor, CologneChip, which uses Yosys for synthesis. During this process, it was discovered that a different block of code in the CRC component failed specifically when synthesized with Yosys.

On the slide, you can see the elegant one-liner that I commented out; it worked correctly with all other vendor tools but not with Yosys. Since we are committed to supporting as many vendor toolchains as possible, I rewrote that one-liner into the five-line implementation shown on the slide.

# 6. Free Tooling

## Slide

- Documentation: wavedrom
- Linter: VSG
- Simulator: GHDL, NVC
- Testbench framework: Vunit
- Synthesis: Yosys
- Implementation: nextpnr

## Narration

By relying heavily on open-source tools, Open Logic can be used and contributed to without requiring expensive vendor licenses. While commercial tools can still be used if available, they are not mandatory. For documentation, Wavedrom is used to generate waveforms. Linting is performed with VSG, while simulations can be run using either GHDL or NVC. Testbenches are written with the VUnit framework. Finally,  synthesis and implementation can be carried out using Yosys and nextpnr, which are used for the CologneChip vendor. This fully open-source toolchain ensures that Open Logic remains accessible to anyone.

# 7. Why use Open Logic?

## Slide

- Don't waste time on developing code that exists
- Don't take risks debugging your own CDC/FIFO/...
- Spend your time on your application ...
	-  ... not on well known Basic Elements
- Why Not? It's free

## Narration

There is little value in spending time developing code that already exists. Doing so also introduces unnecessary risk, particularly when re-implementing complex and error-prone building blocks such as clock domain crossings, FIFOs, and similar infrastructure components.
If you adopt Open Logic it lowers the likelihood that you will need to debug fundamental building blocks. These blocks are widely used, well reviewed, and continuously tested. If an error is discovered, raising an issue on GitHub gives you access to a broad community of engineers who can help diagnose and resolve the problem. Any fix then improves the building block for everyone, not just for you.

Your time is better spent developing your application, not reinventing well-known fundamental components. It is unlikely that a custom FIFO will significantly outperform a mature, widely used implementation. By relying on established, documented, and thoroughly tested building blocks, you reduce risk, improve reliability, and accelerate development, allowing you to focus on what truly differentiates your design and where your expertise matters most.

Why not use Open Logic? It is free, so the barrier to entry is very low. With only a small time investment, you can determine whether it adds value to your workflow.

# 8. Proprietary vs. Open-Source

## Slide

- Number of Reviews
- Testing
- Maintenance

## Narration

Consider proprietary in-house HDL Library. How many engineers have actually reviewed it, and how confident are you that is is free of errors? How often is it tested? Do you even have continuous integration pipelines in place, and if so, how frequently are they run? In many cases, these HDL libraries receive limited scrutiny and limited long-term maintenance.

Open Logic, by contrast, is open source. It is reviewed by a much larger group of engineers and supported by regularly scheduled CI pipelines. When anyone discovers a bug or issue, it is quickly addressed and fixed for the benefit of the entire community. This shared maintenance model significantly reduces the ongoing burden on individual teams.

Infrastructure projects like Open Logic strengthen the entire ecosystem. We should not be competing over who can build the best basic components. Our real value lies in designing and delivering the best applications on top of them.

# 9. Content Overview


# 10. Call to action

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

