# logik
## intro
logik maintains a group of tools to build hw design and the generated design model can run on simulator or on fpga.
logik includes some verification components,
- systemc simulation core libraries, e.g. uvm-systemc
- bfm (drivers/monitors, tlm-bridges/transactors) to interconnect with design
- high level simulation models, virtual analyzers
- scripts for design, documention, devop, and etc

## sysc
systemc core libraries including, 
- systemc 2.3.2
- systemc-uvm
- crave
- fc4sc

## meson
meson is the hw build tool
- meson support mixed build, which takes results from external build system as a build dependence. This way build tool can be reused if it is already available. Especially the cmake modules taken from 'tymonx/logic' to compile HDL files. 
- meson also support the concept of subprojects, and a complex project be partitioned into a hierarchical of directories. Each sub-directory holds a subproject and the parent directory holds the main project. Subprojects can use other subprojects, but all subprojects must reside in the top level subprojects directory. Recursive use of subprojects is not allowed, though, so you can't have subproject a that uses subproject b and have b also use a.

- the project's dir has been arranged as below to use meson
  - dir /hw is for chip level and there is main build file meson.build. 
  - /hw can has several dirs for different chip configuration (named after chip's name). and its /subprojects directory contains IPs (each /hw/ip could be self-contained and could checkout separately)

## ref
/from meson manual/
- meson allows you to take any other meson project and make it a part of your build
- meson has predefined location for subprojects.
  1. All subprojects must be inside subprojects directory. 
  2. The subprojects directory must be at the top level of your project. 
  3. Subproject declaration must be in your top level meson.build.
- Meson modularity
  1. Meson can use the CMake find_package() function to detect dependencies with the builtin Find<NAME>.cmake modules and exported project configurations
  2. As a part of the software configuration, you could get extra data by running external commands.
- unit test:
  - Meson comes with a fully functional unit test system. To use it simply build an executable and then use it in a test.
  - GTest and GMock come as sources that must be compiled as part of your project.
  - This is a valid JUnit XML description of all tests run. It is not streamed out, and is written only once all tests complete running.
  - When tests use the gtest protocol Meson will inject arguments to the test to generate it's own JUnit XML, which Meson will include as part of this XML file.

## ci&cd
continuous integraion:
- nextflow is as jenkins steps,
- jenkins multi-branch task 

continuous deployment:
- for simple projects with a single repo, the deployment can be done by adding 'PROD' tag or making a 'release' branch, which is good enough since the tag/release contains everything required deployment. 
- for complex projects with multiple repo or involve multiple development team, it is often desired to setup a deployment flow. The below outlines the 'cd' process used by logik
  - development progresses are run in parallel with one or more development repo. once ci/regression result shows the deisng is above a certain quality critiera, the repo is branched into 'release'.
  - the change in 'release' branch triggers the deployment process. (e.g. git archieve the release branch and sent it to ostree release area; which is submited and deleted, the release is then checked-out from ostree branch w/ hard-link to save space). the release area is in ostree and is read-only to user.
  - ip can be developped separately and released to the release area (ostree). soc which uses the ip to run simulation takes the ip from reference it at release area instead of checkout it. It is possible for debug porpuse, soc still need to checkout the ip (as a submodule and submit the config into soc debug branch). but it should be only for debug purpose, and once the debug is finished the branch should be deleted. 
  - soc should maintain the same directory structure as in release area if it checkouts ip for debug. whether simulation script takes local ip or released ip is controllable e.g. with a environment variable of ip's base dir.
  - release regression (sign-off for external release) takes source code from ostree release area only. we should make out-of-tree build work, hopefully. 
  - even a single develop repo is used, different team can release their part of ip separately. e.g. using a different release tag/branch, sw and hw team use sw_release and hw_release. And for that branch, only sw part or hw part is released and copied to release area. 
  - ostree is used for release area storage as it supports multiple branch unioned at a single repo, and it uses hard-link to save space. (to check if git can support them as well).

# hw verif

in block level:
- cocotb
- formal

ip/subsystem level: ip is with standard interfaces; subsystem is a group of related ips. 
- sv uvm
- sysc cosim/emu (w/ or w/o qemu)

chip level: whole chip with bootcode/kernel/driver; 
- sysc cosim/emu (w/ or w/o qemu)
- qemu supports cosim mode & firesim mode (see sw/qemu, firesim mode adds interconnected test devices, virtual logic analyzers and etc into dut and controls them w/ tlm-bridge/tlm-transactor_

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

