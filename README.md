# logik
logik maintains a group of scripts to build HDL design with cmake? meson?. The compiled model is for simulation on simulator or on fpga.
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

cmake module/config to compile hw/sw simulations
- conan package for systemc libraries
- cmakelist for hw/sw
- scripts to run compiles/simulation/regression

continuous integraion
- nextflow as jenkins steps,
- jenkins multi-branch task

inspired by,
- tymonx/logic https://github.com/tymonx/logic
- google/opentitan https://github.com/lowRISC/opentitan
