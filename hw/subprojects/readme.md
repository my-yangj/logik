nest github repo at belwo for test dut, reference it as source dir. build and run with devop env provided by logik.

- https://github.com/alexforencich/verilog-axi
- https://github.com/alexforencich/verilog-axis
- https://github.com/alexforencich/verilog-ethernet
- https://github.com/alexforencich/verilog-pcie
- https://github.com/lowRISC/ibex


As I need a dir to put file build.meson so that it be recognized by meson as a subproject, an extra dir need created. e.g. 
```dir
alexforencich-verilog-axi/nested (a dir is created to hold the nested repo, i just put build.meson in the parent dir).
                         /build.meson: top level build for the ip
                         /regress.nf, regress.yml: dir to hold the ci flows and configurtions. 
                                      simulation is run by jenkinsfile at top level, need branch-out to run an ip regression. the main is used for top level regression.
```
