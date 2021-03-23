## Overview
qemu provides a high level model be helpful to s/w development of bsp/driver/os. qemu could also be used for hw/sw cosim. thus it can run in several configurations,
- s/w mode: soc full system model
  - cpu is tcg or virtualized; system devices are in s/w model; missing model may be proxied to host
  
- cosim mode:  and also tlm-bridge/tlm-transactor to connected with dut in simulation/emu
  - include what s/w mode provides with extra co-sim/emu supported
  - only peripherals are permitted in hw dut
  
- firesim mode: 
  - qemu is to control/monitor the simulated/emulated design.
  - qemu devices can be compiled into the simulated/emulated design and connected through tlm bridge/transactor.
  - soc + vip in dut
