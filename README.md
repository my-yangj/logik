# logik
logik maintains a group of scripts to build HDL design with meson. The compiled model is for simulation on simulator or on fpga.
logik includes h/w verification components,
- systemc based simulation core libraries, 
- BFM (drivers and monitors) to connect with hw,
- high level simulation models,
- scripts for documentation, design automation, flow control and etc

systemc core libraries including, 
- systemc 2.3.3
- systemc-uvm
- crave
- fc4sc

Meson
Meson is selected as hw build tool. 
- Meson is in python and easier to learn than cmake
- Meson is modular with lots of build modules available and it supprot subprojects
- Meson buit with unit test
To accomodate it, the project's dir has been made as below, Dir hw is the chips' dir and under hw, there is main build file meson.build. hw can has several chip dirs with each named after chip's name. Subprojects directory contains IPs. All IPs are under subprojects directory, each can be self contained and can checkout separately.
- Meson allows you to take any other Meson project and make it a part of your build
- Meson has predefined location for subprojects.
  1. All subprojects must be inside subprojects directory. 
  2. The subprojects directory must be at the top level of your project. 
  3. Subproject declaration must be in your top level meson.build.
- Meson modularity
  1. Meson can use the CMake find_package() function to detect dependencies with the builtin Find<NAME>.cmake modules and exported project configurations
  2. As a part of the software configuration, you could get extra data by running external commands.
  
Unit test:
- Meson comes with a fully functional unit test system. To use it simply build an executable and then use it in a test.
- GTest and GMock come as sources that must be compiled as part of your project.
- This is a valid JUnit XML description of all tests run. It is not streamed out, and is written only once all tests complete running.
- When tests use the gtest protocol Meson will inject arguments to the test to generate it's own JUnit XML, which Meson will include as part of this XML file.

continuous integraion
- nextflow as jenkins steps,
- jenkins multi-branch task

inspired by,
- tymonx/logic https://github.com/tymonx/logic
- google/opentitan https://github.com/lowRISC/opentitan
