# logik
logik maintains a group of tools to build hw design with meson. The generated design model can run on simulator or on fpga.
logik includes some verification components,
- systemc simulation core libraries, e.g. uvm-systemc
- bfm (drivers/monitors, tlm-bridges/transactors) to interconnect with design
- high level simulation models, virtual analyzers
- scripts for design, documention, devop, and etc

systemc core libraries including, 
- systemc 2.3.2
- systemc-uvm
- crave
- fc4sc

meson is our hw build tool and meson support mixed build (take results from other build system as a build dependence), so there are other build system has been involved. such as cmake from (logic)

To use meson, the project's dir has been arranged as below, dir /hw is for chip level and there is main build file meson.build. 
/hw can has several dirs for different chip configuration (named after chip's name). /subprojects directory contains IPs, each /hw/ip could be self-contained and could checkout separately.

/from meson manual/
- meson allows you to take any other meson project and make it a part of your build
- meson has predefined location for subprojects.
  1. All subprojects must be inside subprojects directory. 
  2. The subprojects directory must be at the top level of your project. 
  3. Subproject declaration must be in your top level meson.build.
- Meson modularity
  1. Meson can use the CMake find_package() function to detect dependencies with the builtin Find<NAME>.cmake modules and exported project configurations
  2. As a part of the software configuration, you could get extra data by running external commands.
  
unit test:
- Meson comes with a fully functional unit test system. To use it simply build an executable and then use it in a test.
- GTest and GMock come as sources that must be compiled as part of your project.
- This is a valid JUnit XML description of all tests run. It is not streamed out, and is written only once all tests complete running.
- When tests use the gtest protocol Meson will inject arguments to the test to generate it's own JUnit XML, which Meson will include as part of this XML file.

continuous integraion:
- nextflow as jenkins steps,
- jenkins multi-branch task

# hw verif

in block level:
- cocotb
- formal

ip/subsystem level: ip is with standard interfaces; subsystem is a group of related ips. 
- sv uvm
- sysc cosim
- qemu cosim/emu

chip level: whole chip with bootcode/kernel/driver; 
- sysc cosim
- qemu cosim/emu
  cosim mode & firesim mode (see sw/qemu).
  firesim mode adds interconnected test devices, virtual logic analyzers and etc which could controlled them w/ tlm-bridge/tlm-transactor

tlm-brige:

tlm-transactor:
- target side, tunnel/bridge a protocol e.g. axi into tilelink
- transport tilelink over ethernet/pcie e.g. ommixtend
- in host side, analyze payloads and restore them into original protocols. 

(xtor is for fpga/emu cosim; bridge is s/w cosim which dont require a signal transport e.g. pcie/ethernet)

# sw

qemu:
- qemu emulates cpu, and other devices 
- qemu emulates cpu, but other devices are thourgh tlm cosim/coemu
- qemu emulates nothing, but controls test devices/models connected with /embedded in design, e.g. connected uart or bus monitor.

toolchain:

dtb:
- generated from hw

kernel:
- rootbuild

device driver:

# doc
- projects' doc are in sphinx (extracted from phython, or converted from doxygen)
- website is in hugo (also static but more goodlooking?)

# reference
inspired by,
- tymonx/logic https://github.com/tymonx/logic
- google/opentitan https://github.com/lowRISC/opentitan
- xilinx/systemctlm https://github.com/Xilinx/libsystemctlm-soc

