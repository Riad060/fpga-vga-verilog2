---
layout: home
title: FPGA VGA Driver Project
tags: fpga vga verilog
categories: demo
---

Hello my name is Riad and welcome to my FPGA VGA project, this is where hardware level graphics come alive without any CPU or GPU. My Demo shows how my custom verilog logic can run a real monitor and also display fully generated images in real time.


## **Template VGA Design**
### **Project Set-Up**
My Project was developed in Vivado using the standard FPGA design flow, it creates a new peoject also adds the verilog source files,instantiating the clock wizard IP and also applying the Basys3 constraint file for the VGA output. After i wrote and integrated the Vga timing module and top level design the project was then sysntesised, implemented and finally programmed into the board. My screenshot below the whole project overview and setup.
<img width="976" height="586" alt="Riadoverview" src="https://github.com/user-attachments/assets/85144bbf-dcdc-4364-9654-048bd2a078ac" />


### **Template Code**
Basically the template code is splinto into three main verliogs.The first one is the **VGA Timing Module**(`VGASync`) which makes 640x480 @ 60Hz video timing. It also uses horizontal and vertical counters that cycle through every pixel and makes `hsync` and `vsync` at the correct times and outputs the current `row` and `col` values together also with a `vid_on` signal that tells you when the pixel is inside visable area.


Now the second module is the **colour/graphics** module which was originally 'ColourStripes'. This one takes `row` and `col` as inputs and then decides what the RGB value should output for every pixel. In the template design it basically divides the screen into vertical bands and assigns colours which is a pretty useful test pattern for checking that tje timing and colour outputs are correct

The final part is the **top-level module** (`VGATop`). It like connects the Basys3 clock to the Clock Wizard and instantiates `VGASync` and the colour module and then routes the `vgaRed` / `vgaGreen` and `vgaBlue` buses plus `hSync` and `vSync` out to the actual FPGA pins that are defined in the XDC file. Alltogether these modules form a fully complete VGA pipeline implemented purely in hardware.

### **Simulation**

To verify the template design, I first ran a behavioural simulation in Vivado. The testbench drives a clock and reset into the top-level design and lets the counters run for several frame periods. In the waveform viewer I could see the 25 MHz pixel clock, the `hsync` and `vsync` pulses at the expected intervals, and the `row`/`col` values sweeping through the visible 640×480 region. The RGB signals also changed as the pixel position moved between the different colour bands.

### **Simulation**
<img width="1757" height="975" alt="image" src="https://github.com/user-attachments/assets/a1cd85be-00be-46d6-8a35-032fe078fb58" />

To verify that my VGA was functioning correctly i had to run a behavioural simulation within the provided template modules before i had to move on to my own design. The simulation helped me to understand and observe the essential VGA timing signals like hsync/vsync/row/col and the 25MHz pixel clock. During the testbench run I checked that col counted from 0 - 639 and row counted from 0 - 479 during the active display area also confirming that the horizontal and vertical timings matched the VGA 640x480 standard. I also had to verify that the vid_on signal only went high during visible region allowing the colour generator would only drive pixels insidde the display window.

A key detail in my simulation was that the timing pulse must happen at the correct positions within the frame. The simulation waveform clearly shows the short hsync pulse every line and the vsync pulse every frame whcih then confirmed the correctness of the sync pulse/front porch and back porch paramatres. This step had to be done before synthesising the design because incorrect timing would result in a blank or unstable image on your monitor
### **Synthesis**
Describe the synthesis and implementation processes. Consider including 1/2 useful screenshot(s). Guideline: 1/2 short paragraphs.
### **Demonstration**
Perhaps add a picture of your demo. Guideline: 1/2 sentences.

## **My VGA Design Edit**
Introduce your own design idea. Consider how complex/achievabble this might be or otherwise. Reference any research you do online (use hyperlinks).
### **Code Adaptation**
Briefly show how you changed the template code to display a different image. Demonstrate your understanding. Guideline: 1-2 short paragraphs.
### **Simulation**
Show how you simulated your own design. Are there any things to note? Demonstrate your understanding. Add a screenshot. Guideline: 1-2 short paragraphs.
### **Synthesis**
Describe the synthesis & implementation outputs for your design, are there any differences to that of the original design? Guideline 1-2 short paragraphs.
### **Demonstration**
If you get your own design working on the Basys3 board, take a picture! Guideline: 1-2 sentences.

## **More Markdown Basics**
This is a paragraph. Add an empty line to start a new paragraph.

Font can be emphasised as *Italic* or **Bold**.

Code can be highlighted by using `backticks`.

Hyperlinks look like this: [GitHub Help](https://help.github.com/).

A bullet list can be rendered as follows:
- vectors
- algorithms
- iterators

Images can be added by uploading them to the repository in a /docs/assets/images folder, and then rendering using HTML via githubusercontent.com as shown in the example below.

<img src="https://raw.githubusercontent.com/melgineer/fpga-vga-verilog/main/docs/assets/images/VGAPrjSrcs.png">
