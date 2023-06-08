# Caravel SoC FPGA
[Boledu](https://www.boledu.org/) proposed a Caravel SoC FPGA validation platform which was ported from [Efabless Caravel harness SoC](https://caravel-harness.readthedocs.io/en/latest/#efabless-caravel-harness-soc) project (Google Open Source Silicon) and could be ran on [Xilinx PYNQ-Z2](https://www.xilinx.com/support/university/xup-boards/XUPPYNQ-Z2.html) FPGA board.  
The Boledu [Caravel SoC](https://github.com/bol-edu/caravel-soc) ([MPW8-C](https://github.com/efabless/caravel_user_project/tree/mpw-8c) code base) was removed all SKY PDK dependencies to provide quickly logic simulation for development. Two software debugging enhances were proposed and implemented. Firstly, an open source [GDBWave](https://tomverbeure.github.io/2022/02/20/GDBWave-Post-Simulation-RISCV-SW-Debugging.html) was integrated into Caravel SoC design flow. The GDBWave parses the waveform after RTL simulation to manipulate GDB debugging with VexRiscv CPU. Secondly, a proposed [Riscv-Tracer](https://github.com/bol-edu/caravel-soc/tree/cpu-trace) was helped to translate waveform display to RISC-V instructions representation.  
The Boledu [Caravel SoC FPGA](https://github.com/bol-edu/caravel-soc_fpga) was ported from [Caravel SoC](https://github.com/bol-edu/caravel-soc) with a little SoC system revisions. Caravel users can quickly prototype their user project design on Xilinx FPGA board. The Xilinx Vitis provides [Xilinx Vivado Simulator](https://github.com/Xilinx/xup_fpga_vivado_flow/blob/main/slides/12_Vivado%20Design%20Flow.pdf) (XSIM) and [Xiliinx Vivado](https://www.xilinx.com/products/design-tools/vivado.html) hardware block design tool for Caravel SoC FPGA bitstream generation. Finally, Caravel users can continue to validate their design on Boledu online FPGA board with Jupyter Notebook testbench.

![boledu dev flow](https://github.com/bol-edu/caravel-soc_fpga/assets/98332019/01126c0e-d6b6-4715-8fb6-dec622adf28d)

To emulate the Efabless Caravel harness SoC simulation environment, we implemented [4 simple designs](https://github.com/bol-edu/caravel-soc_fpga/tree/main/vivado/vitis_prj) as in the figure below.
1. ResetControl: A memory-mapped-io to control Caravel reset pin. 
2. read_romcode: read the firmware hex data from PS/DDR memory to FPGA BRAM.
3. spiflash: It emulates spiflash device read behavior. It reads data from BRAM.
4. caravel_ps: It allows the PS side to use memory-mapped IO to read/write mprj pins.  

The Caravel Verilog testbench code can be easily translated to Jupyter Notebook Python testbench code as illustrated in [caravel_fpga.ipynb]( https://github.com/bol-edu/caravel-soc_fpga/blob/main/vivado/jupyter_notebook/caravel_fpga.ipynb)

<p align="center"><img src="https://github.com/bol-edu/caravel-soc_fpga/assets/98332019/66b47711-5bfc-4178-9a62-0a64ce5ecf17" width=70%></p>

Any question/idea of Caravel SoC FPGA can be posted in [Discussions](https://github.com/bol-edu/caravel-soc_fpga/discussions).

Acknowledgement for Project:  
[Patrick Lin](mailto:patrickxlin@gmail.com)/[patrick-lin-git](https://github.com/patrick-lin-git), [Willy Chiang](mailto:cwenpin@gmail.com), [Tony Ho](mailto:tonyho722@gmail.com), [Allen Chang](mailto:mailggwhc@gmail.com), [Ian Liu](mailto:IanLiu@vianextech.com), [Bow Chao](mailto:var256@gmail.com)

[Video Demonstration on Youtube](https://youtu.be/HaaOUdxiA40)  
<img src="https://github.com/bol-edu/caravel-soc_fpga/assets/98332019/5b504085-bc4f-4e3c-b685-5b38ac79b888" width=60%>

## Toolchain Prerequisites
* [Ubuntu 20.04+](https://releases.ubuntu.com/focal/)
* [Xilinx Vitis 2022.1](https://www.xilinx.com/support/download/index.html/content/xilinx/en/downloadNav/vivado-design-tools/2022-1.html) (builtin XSIM and Vivado)
* [RISC-V GCC Toolchains rv32i-4.0.0](https://github.com/stnolting/riscv-gcc-prebuilt)
* [GTKWave v3.3.103](https://gtkwave.sourceforge.net/)

## Directory Structure
```
├── firmware                # caravel firmware libraries
├── rtl                     # caravel rtl designs
│   ├── header              # headers
│   ├── soc-efabless        # efabless caravel soc
│   ├── soc                 # boledu revised caravel soc
│   ├── user                # user project designs
├── testbench               # caravel testbenches
│   ├── counter_la          # counter with logic analyzer interface
│   ├── counter_wb          # counter with wishbone interface
│   └── gcd_la              # gcd with logic analyzer interface
├── vip                     # caravel verification ip
└── vivado                  # boledu caravel soc fpga
```

## Overview of Caravel SoC FPGA
* [Prepare a Xilinx Vitis on Ubuntu Machine](https://github.com/bol-edu/caravel-soc_fpga#prepare-a-xilinx-vitis-on-ubuntu-machine)
* [Setup RISC-V GCC Toolchains and GTKWave](https://github.com/bol-edu/caravel-soc_fpga#setup-risc-v-gcc-toolchains-and-gtkwave)
* [Git Clone Caravel SoC FPGA](https://github.com/bol-edu/caravel-soc_fpga#git-clone-caravel-soc-fpga)
* [Run Xilinx Vivado Simulation of Caravel SoC FPGA](https://github.com/bol-edu/caravel-soc_fpga#run-xilinx-vivado-simulation-of-caravel-soc-fpga)
* [Generate Caravel SoC FPGA Bitstream from Xilinx Vivado](https://github.com/bol-edu/caravel-soc_fpga#generate-caravel-soc-fpga-bitstream-from-xilinx-vivado)
* [Test Caravel User Project Design on FPGA Board with Jupyter Notebook](https://github.com/bol-edu/caravel-soc_fpga#test-caravel-user-project-design-on-fpga-board-with-jupyter-notebook)
* [Appendix: Revision History from Efabless Caravel harness SoC to Caravel SoC FPGA](https://github.com/bol-edu/caravel-soc_fpga#appendix-revision-history-from-efabless-caravel-harness-soc-to-caravel-soc-fpga)
* [Appendix: Boledu Vitis Machine](https://github.com/bol-edu/caravel-soc_fpga/blob/main/README.md#appendix-boledu-vitis-machine)
* [Appendix: AWS EC2 Vitis Subscription](https://github.com/bol-edu/caravel-soc_fpga/blob/main/README.md#appendix-aws-ec2-vitis-subscription)
* [Appendix: Reference Documents](https://github.com/bol-edu/caravel-soc_fpga/blob/main/README.md#appendix-reference-documents)

## Quick Start
If you want to experience FPGA board validation with prebuilt Caravel counter user project FPGA bitstream and Jupyter Notebook testbench, you can hand-on the [last FPGA validation step](https://github.com/bol-edu/caravel-soc_fpga#test-caravel-user-project-design-on-fpga-board-with-jupyter-notebook). 

## Prepare a Xilinx Vitis on Ubuntu Machine
The Xilinx Vitis needs minimum 32 GB system memory (64 GB is recommended). You can in-house setup Vitis on your Ubuntu machine or apply a ready for using Vitis machine from Boledu (free) / AWS EC2 (charge).

**In-house setup (deploying with hours)**
* Install necessary dependencies before Vitis installation: `sudo apt install libtinfo5 libncurses5 -y`
* Follow offical installation guide: https://docs.xilinx.com/r/2022.1-English/ug1400-vitis-embedded/Installation-Requirements  
* Add below lines to `/home/<user>/.bashrc` after completing Vitis installation  
`source <Vitis_install_path>/Xilinx/Vitis/2022.1/settings64.sh`  
`source <Vitis_install_path>/xilinx/xrt/setup.sh`  
* Before running Vitis `source /home/<user>/.bashrc`

**Boledu presetup (no deploying)**
* Limited quotas: 20 users
* Provide both online [Vitis machine user flow](https://github.com/bol-edu/caravel-soc_fpga/blob/main/README.md#appendix-boledu-vitis-machine) and [PYNQ-Z2 FPGA board user flow](https://github.com/bol-edu/caravel-soc_fpga/tree/main#validation-on-pynq-fpga-board)
* Fill out [application form](https://docs.google.com/forms/d/e/1FAIpQLScZQZXyrtWZiT6r6_HkJhWV-VPNmRv6qAE5ehtmu2Qz-71V4w/viewform?usp=share_link) and [feedback your comments of Caravel SoC FPGA](https://github.com/bol-edu/caravel-soc_fpga/discussions)

**AWS EC2 Vitis subscription (deploying within minutes but charge)**  

The detailed subscription diagrams flow are described in [Appendix](https://github.com/bol-edu/caravel-soc_fpga/blob/main/README.md#appendix-aws-ec2-vitis-subscription).

## Setup RISC-V GCC Toolchains and GTKWave
```console
$ sudo apt update
$ sudo apt install gtkwave -y
$ sudo wget -O /tmp/riscv32-unknown-elf.gcc-12.1.0.tar.gz https://github.com/stnolting/riscv-gcc-prebuilt/releases/download/rv32i-4.0.0/riscv32-unknown-elf.gcc-12.1.0.tar.gz
$ sudo mkdir /opt/riscv
$ sudo tar -xzf /tmp/riscv32-unknown-elf.gcc-12.1.0.tar.gz -C /opt/riscv
$ echo 'export PATH=$PATH:/opt/riscv/bin' >> ~/.bashrc
$ source ~/.bashrc
```
## Git Clone Caravel SoC FPGA
```console
$ sudo apt install git -y
$ git clone https://github.com/bol-edu/caravel-soc_fpga ~/caravel-soc_fpga
```
## Run Xilinx Vivado Simulation of Caravel SoC FPGA
Caravel users can develop their own user project design integrated with Caraver SoC FPGA and use Xilinx Vivado simulator (XSIM) to validate design's behavior. For counter_la design example, you can find a design file path `../../rtl/user/user_proj_example.counter.v` was listed within `include.rtl.list.xsim`. You can change the design file path to your new user project design location, then rewrite reference RISC-V firmware `counter_la.c` and Verilog testbench `counter_la_tb.v` to test your new user project design. The shell script commands of `run_xsim` are also needed to align your rewritten firmware and testbench files which were used in new user project design.  
The compiled RISC-V firmwares [counter_la.hex](https://github.com/bol-edu/caravel-soc_fpga/blob/main/testbench/counter_la/counter_la.hex), [counter_wb.hex](https://github.com/bol-edu/caravel-soc_fpga/blob/main/testbench/counter_wb/counter_wb.hex) and [gcd_la.hex](https://github.com/bol-edu/caravel-soc_fpga/blob/main/testbench/gcd_la/gcd_la.hex) were generated while exeuting their `run_xsim`. They were separately used by testbenches counter_la_tb.v, counter_wb_tb.v and gcd_la_tb.v. The compiled RISC-V firmwares also can be reused by later Jupyter Notebook testbench running on FPGA board.

* Counter with (LA) logic analyzer interface  

Files of counter_la for running Xilinx XSIM:  
[caravel-soc_fpga/testbench/counter_la/counter_la.c](https://github.com/bol-edu/caravel-soc_fpga/blob/main/testbench/counter_la/counter_la.c)  
[caravel-soc_fpga/testbench/counter_la/counter_la_tb.v](https://github.com/bol-edu/caravel-soc_fpga/blob/main/testbench/counter_la/counter_la_tb.v)  
[caravel-soc_fpga/testbench/counter_la/include.rtl.list.xsim](https://github.com/bol-edu/caravel-soc_fpga/blob/main/testbench/counter_la/include.rtl.list.xsim)  
[caravel-soc_fpga/testbench/counter_la/run_xsim](https://github.com/bol-edu/caravel-soc_fpga/blob/main/testbench/counter_la/run_xsim)  

Use GTKWave to view simulated counter_la waveform
```console
$ cd ~/caravel-soc_fpga/testbench/counter_la/
$ ./run_xsim
$ gtkwave waveform.gtkw
```
![counter_la](https://github.com/bol-edu/caravel-soc_fpga/assets/98332019/41ae723e-8214-424b-9e66-171bce87829e)

Detailed [counter_la.log](https://github.com/bol-edu/caravel-soc_fpga/files/11592235/counter_la.log) of run_xsim  

* Counter with (WB) wishbone interface  

Files of counter_wb for running Xilinx XSIM:  
[caravel-soc_fpga/testbench/counter_wb/counter_wb.c](https://github.com/bol-edu/caravel-soc_fpga/blob/main/testbench/counter_wb/counter_wb.c)  
[caravel-soc_fpga/testbench/counter_wb/counter_wb_tb.v](https://github.com/bol-edu/caravel-soc_fpga/blob/main/testbench/counter_wb/counter_wb_tb.v)  
[caravel-soc_fpga/testbench/counter_wb/include.rtl.list.xsim](https://github.com/bol-edu/caravel-soc_fpga/blob/main/testbench/counter_wb/include.rtl.list.xsim)  
[caravel-soc_fpga/testbench/counter_wb/run_xsim](https://github.com/bol-edu/caravel-soc_fpga/blob/main/testbench/counter_wb/run_xsim)  

Use GTKWave to view simulated counter_wb waveform
```console
$ cd ~/caravel-soc_fpga/testbench/counter_wb/
$ ./run_xsim
$ gtkwave waveform.gtkw
```
![counter_wb](https://github.com/bol-edu/caravel-soc_fpga/assets/98332019/b7315445-ec13-4783-b612-617b0dcdbc0c)

Detailed [counter_wb.log](https://github.com/bol-edu/caravel-soc_fpga/files/11592238/counter_wb.log) of run_xsim  

* GCD with (LA) logic analyzer interface  

Files of gcd_la for running Xilinx XSIM:  
[caravel-soc_fpga/testbench/gcd_la/gcd_la.c](https://github.com/bol-edu/caravel-soc_fpga/blob/main/testbench/gcd_la/gcd_la.c)  
[caravel-soc_fpga/testbench/gcd_la/gcd_la_tb.v](https://github.com/bol-edu/caravel-soc_fpga/blob/main/testbench/gcd_la/gcd_la_tb.v)  
[caravel-soc_fpga/testbench/gcd_la/include.rtl.list.xsim](https://github.com/bol-edu/caravel-soc_fpga/blob/main/testbench/gcd_la/include.rtl.list.xsim)  
[caravel-soc_fpga/testbench/gcd_la/run_xsim](https://github.com/bol-edu/caravel-soc_fpga/blob/main/testbench/gcd_la/run_xsim)  

Use GTKWave to view simulated gcd_la waveform
```console
$ cd ~/caravel-soc_fpga/testbench/gcd_la/
$ ./run_xsim
$ gtkwave waveform.gtkw
```
![gcd_la](https://github.com/bol-edu/caravel-soc_fpga/assets/98332019/f2974093-6956-4573-9c72-4cae0f684dd8)

Detailed [gcd_la.log](https://github.com/bol-edu/caravel-soc_fpga/files/11592241/gcd_la.log) of run_xsim  

## Generate Caravel SoC FPGA Bitstream from Xilinx Vivado
After design validation through Vivado simulation, Caraver SoC FPGA and its integrated user project design can be synthesized to FPGA bitstream and continue to be validated on PYNQ-Z2 FPGA board with Jupyter Notebook testbench. Before running Xilinx Vivado synthesis , you need to setup PYNQ-Z2 board files.

### PYNQ-Z2 Board Files
* In-house Environment
```console
$ sudo wget -O /tmp/pynq-z2.zip https://dpoauwgwqsy2x.cloudfront.net/Download/pynq-z2.zip
$ sudo unzip /tmp/pynq-z2.zip -d <Vitis_install_path>/Xilinx/Vivado/2022.1/data/boards/board_files/
```
* Boledu Environment

The PYNQ-Z2 board files are already set.

* AWS EC2 Vitis Environment
```console
$ sudo wget -O /tmp/pynq-z2.zip https://dpoauwgwqsy2x.cloudfront.net/Download/pynq-z2.zip
$ sudo unzip /tmp/pynq-z2.zip -d /tools/Xilinx/Vivado/2022.1/data/boards/board_files/
```

### Xilinx Vivado Synthesis
The `run_vivado` shell script will build up whole Xilinx Vivado project and generate default Caravel counter user project FPGA bitstream.
```console
$ cd ~/caravel-soc_fpga/vivado
$ ./run_vivado
```
After finishing `run_vivado`, you can see below messages and find generated `vivado.log` and `timing_report.log`. You also need to check is there any timing violation existence inside `timing_report.log`. Default `run_vivado` invokes `vvd_caravel_fpga_10mhz.tcl` to target 10MHz FPGA synthesis. If you have timing issue or invalid FPGA test result, you can try to tune user project design architecture or change `run_vivado` to invoke `vvd_caravel_fpga_1mhz.tcl` to target 1MHz FPGA synthesis.

```
open_run: Time (s): cpu = 00:00:11 ; elapsed = 00:00:09 . Memory (MB): peak = 3493.906 ; gain = 264.785 ; free physical = 56823 ; free virtual = 62238
# report_timing_summary -file timing_report.log
INFO: [Timing 38-91] UpdateTimingParams: Speed grade: -1, Delay Type: min_max.
INFO: [Timing 38-191] Multithreading enabled for timing update using a maximum of 8 CPUs
# exit
INFO: [Common 17-206] Exiting Vivado at Tue May 23 17:31:47 2023...
======================================================================
                           vivado complete
======================================================================
```

If needed you can change [run_vivado](https://github.com/bol-edu/caravel-soc_fpga/blob/main/vivado/run_vivado):23 to target 1MHz FPGA synthesis.
```
vivado -source vvd_caravel_fpga_1mhz.tcl -mode tcl
```

PYNQ-Z2 FPGA bitstream will be updated in caravel-soc_fpga/vivado/jupyter_notebook/
```console
$ ls -alF jupyter_notebook/
total 4260
drwxrwxr-x 2 hls05 hls05    4096  5  18 06:14 ./
drwxrwxr-x 8 hls05 hls05    4096  5  18 06:31 ../
-rw-rw-r-- 1 hls05 hls05 4045676  5  18 06:34 caravel_fpga.bit
-rw-rw-r-- 1 hls05 hls05  280106  5  18 06:34 caravel_fpga.hwh
-rw-rw-r-- 1 hls05 hls05   10182  5  18 06:14 caravel_fpga.ipynb
```  
`caravel_fpga.bit` is FPGA bitstream generated by Vivado after executing `run_vivado`  
`caravel_fpga.hwh` is FPGA (HWH) hardware description file generated by Vivado after executing `run_vivado`  
`caravel_fpga.ipynb` is a Jupyter Notebook testbench example code for counter_la/counter_wb  

### Change Counter User Project Design to GCD User Project Design
Copy gcd user project design source to vivado user source directory and set environment virable `USER_DESIGN_FILE`. Then, you can re-run `run_vivado` to generate Caravel gcd user project FPGA bitstream.
```console
$ cp ~/caravel-soc_fpga/rtl/user/user_proj_example.gcd.v ~/caravel-soc_fpga/vivado/vvd_srcs/caravel_soc/rtl/user
$ export USER_DESIGN_FILE=user_proj_example.gcd.v
```
## Test Caravel User Project Design on FPGA Board with Jupyter Notebook
Before validating your Caravel user project design on FPGA board with Jupyter Notebook testbench, you should confirm the checklist of (1) compiled RISC-V firmwares, (2) FPGA bitstream, (3) FPGA HWH file and (4) Jupyter Notebook testbench example code.

### Checklist
For (prebuilt) Caravel counter user project:
* [counter_la.hex](https://github.com/bol-edu/caravel-soc_fpga/blob/main/testbench/counter_la/counter_la.hex)
* [counter_wb.hex](https://github.com/bol-edu/caravel-soc_fpga/blob/main/testbench/counter_wb/counter_wb.hex)
* [caravel_fpga.bit](https://github.com/bol-edu/caravel-soc_fpga/blob/main/vivado/jupyter_notebook/caravel_fpga.bit)
* [caravel_fpga.hwh](https://github.com/bol-edu/caravel-soc_fpga/blob/main/vivado/jupyter_notebook/caravel_fpga.hwh)
* [caravel_fpga.ipynb](https://github.com/bol-edu/caravel-soc_fpga/blob/main/vivado/jupyter_notebook/caravel_fpga.ipynb)

For Caravel gcd user project:
* [gcd_la.hex](https://github.com/bol-edu/caravel-soc_fpga/blob/main/testbench/gcd_la/gcd_la.hex)
* caravel_fpga.bit (follow [above instructions](https://github.com/bol-edu/caravel-soc_fpga#change-counter-user-project-design-to-gcd-user-project-design) to generate)
* caravel_fpga.hwh (follow [above instructions](https://github.com/bol-edu/caravel-soc_fpga#change-counter-user-project-design-to-gcd-user-project-design) to generate)
* caravel_fpga.ipynb (modification from [example code](https://github.com/bol-edu/caravel-soc_fpga/blob/main/vivado/jupyter_notebook/caravel_fpga.ipynb))

Modify default counter_la.hex firmware read
```
fiROM = open("counter_la.hex", "r+")
#fiROM = open("counter_wb.hex", "r+")
```
to gcd_la.hex firmware read
```
#fiROM = open("counter_la.hex", "r+")
#fiROM = open("counter_wb.hex", "r+")
fiROM = open("gcd_la.hex", "r+")
```
### Validation on PYNQ FPGA Board
You can validate designed Caravel user project on your PYNQ-Z2 FPGA board or use presetup boards from Boledu OnlineFPGA ([application form](https://docs.google.com/forms/d/e/1FAIpQLScZQZXyrtWZiT6r6_HkJhWV-VPNmRv6qAE5ehtmu2Qz-71V4w/viewform)). We demonstrate PYNQ-Z2 FPGA board user flow to explain the validation steps.

* OnlineFPGA login
```
######################################################
#                                                    #
#       Welcome to BoLedu's OnlineFPGA service       #
#        Renting OnlineFPGA from service menu        #
#                                                    #
#       Shutdown: TPE(UTC+8) 6:00am to 7:00am        #
#                                                    #
#            email: onlinefpga@gmail.com             #
#                                                    #
######################################################

[1] login evaluation
[0] exit OnlineFPGA service

please enter your option:
>> 1
please enter your register email:
>> caravel.user@gmail.com
please enter your password:
>> *****
```
* OnlineFPGA rent - PYNQ-Z2 board
```
[1] list all online device boards
[2] rent a specified device board
[3] return your rented device board
[0] exit OnlineFPGA service

please enter your option:
>> 1
all online device boards
{'device': 'pynq_01',  'status': 'available'}
{'device': 'pynq_02',  'status': 'unknown'}
{'device': 'pynq_03',  'status': 'available'}
{'device': 'pynq_04',  'status': 'available'}
{'device': 'pynq_05',  'status': 'used'}
{'device': 'pynq_06',  'status': 'available'}
{'device': 'pynq_07',  'status': 'available'}
{'device': 'pynq_09',  'status': 'available'}
{'device': 'pynq_11',  'status': 'available'}
{'device': 'pynq_12',  'status': 'available'}
{'device': 'pynq_13',  'status': 'unknown'}
{'device': 'pynq_14',  'status': 'available'}
{'device': 'pynq_15',  'status': 'available'}
{'device': 'pynq_16',  'status': 'available'}
{'device': 'pynq_17',  'status': 'available'}
{'device': 'pynq_18',  'status': 'available'}
{'device': 'u50_01',   'status': 'available', 'user': 1, 'queue_user': 0, 'u50_board': 0, 'tenant': 9}
{'device': 'u50_02',   'status': 'available', 'user': 0, 'queue_user': 0, 'u50_board': 1, 'tenant': 7}
{'device': 'u50_03',   'status': 'available', 'user': 1, 'queue_user': 1, 'u50_board': 0, 'tenant': 8}
{'device': 'u50_04',   'status': 'available', 'user': 0, 'queue_user': 0, 'u50_board': 1, 'tenant': 7}

[1] list all online device boards
[2] rent a specified device board
[3] return your rented device board
[0] exit OnlineFPGA service

please enter your option:
>> 2

[1] pynq
[2] u50

please enter your option:
>> 1

[1] rent a device board by choice
[2] rent a device board by assignment

please enter your option:
>> 1
all online device boards
{'device': 'pynq_01',  'status': 'available'}
{'device': 'pynq_02',  'status': 'unknown'}
{'device': 'pynq_03',  'status': 'available'}
{'device': 'pynq_04',  'status': 'available'}
{'device': 'pynq_05',  'status': 'used'}
{'device': 'pynq_06',  'status': 'available'}
{'device': 'pynq_07',  'status': 'available'}
{'device': 'pynq_09',  'status': 'available'}
{'device': 'pynq_11',  'status': 'available'}
{'device': 'pynq_12',  'status': 'available'}
{'device': 'pynq_13',  'status': 'unknown'}
{'device': 'pynq_14',  'status': 'available'}
{'device': 'pynq_15',  'status': 'available'}
{'device': 'pynq_16',  'status': 'available'}
{'device': 'pynq_17',  'status': 'available'}
{'device': 'pynq_18',  'status': 'available'}

please enter pynq device name which you want to rent:
>> pynq_15
device pynq_15 is available
do you want to rent this device? (y/n)
>> y
user caravel.user@gmail.com rented device pynq_15 successfully
jupyter web ip port is 218.103.115.199:21500, web passwd is uqcseg and timeup at 05/24/2023 01:46:56
```
* Web login `218.103.115.199:21500` with passwd `uqcseg`
![jupyter01](https://github.com/bol-edu/caravel-soc_fpga/assets/98332019/e652c674-a4fc-455e-a8ce-b8b9ab7a4fcd)

* New -> Folder -> Rename default `Untitled Folder` Folder -> `ipy_fpga` Folder
![jupyter02](https://github.com/bol-edu/caravel-soc_fpga/assets/98332019/1457195c-99d5-4656-813c-464e5e300c7d)

* Click into `ipy_fpga` Folder -> Upload all tests to wait queue from local -> Click each blue Upload bar to transfer
![jupyter03](https://github.com/bol-edu/caravel-soc_fpga/assets/98332019/98eaf5fa-baeb-44f5-bc20-4917558e0f27)

* Complete transfer
![jupyter04](https://github.com/bol-edu/caravel-soc_fpga/assets/98332019/286f1aba-c689-4161-b68d-c692e5c58cb1)

* Click `caravel_fpga.ipynb` to open new tab and run Jupyter Notebook test
![jupyter05](https://github.com/bol-edu/caravel-soc_fpga/assets/98332019/ce044ac5-2674-4b14-bb0a-f897bb7b6e47)

* Run Jupyter Notebook cell-by-cell and use default `counter_la.hex` firmware
![jupyter06](https://github.com/bol-edu/caravel-soc_fpga/assets/98332019/15d841e3-d558-46e4-9b9c-c3a078963be0)

* Get final `mprj value = 0xab51` (mapping to 16-bit reg_mprj_datal in [counter_la.c](https://github.com/bol-edu/caravel-soc_fpga/blob/main/testbench/counter_la/counter_la.c):128)
![jupyter07](https://github.com/bol-edu/caravel-soc_fpga/assets/98332019/bd4cb119-e39c-42ce-9213-650e2e5dc2e3)

As demonstration, you should able to test Caravel counter with counter_wb.hex firmware, Caravel gcd with gcd_la.hex firmware and even your new Caravel user project design.

## Appendix: Revision History from Efabless Caravel harness SoC to Caravel SoC FPGA
### Revision from Caravel SoC (No PDK) to Caravel SoC FPGA
In Xilinx FPGA revision stage, we had follow modifications to pass Xilinx compiler check without changing any logic behavior of Caravel.

**`default_nettype none to wire (checked by Xilinx xvlog)**  
* [caravel-soc_fpga/vip/tbuart.v](https://github.com/bol-edu/caravel-soc_fpga/blob/main/vip/tbuart.v):1  
* [caravel-soc_fpga/vip/spiflash.v](https://github.com/bol-edu/caravel-soc_fpga/blob/main/vip/spiflash.v):1  
* [caravel-soc_fpga/rtl/user/user_project_wrapper.v](https://github.com/bol-edu/caravel-soc_fpga/blob/main/rtl/user/user_project_wrapper.v):16  
* [caravel-soc_fpga/rtl/user/user_proj_example.counter.v](https://github.com/bol-edu/caravel-soc_fpga/blob/main/rtl/user/user_proj_example.counter.v):16  
* [caravel-soc_fpga/rtl/user/user_proj_example.gcd.v](https://github.com/bol-edu/caravel-soc_fpga/blob/main/rtl/user/user_proj_example.gcd.v):16  
* [caravel-soc_fpga/rtl/soc/gpio_control_block.v](https://github.com/bol-edu/caravel-soc_fpga/blob/main/rtl/soc/gpio_control_block.v):58  
* [caravel-soc_fpga/rtl/soc/housekeeping.v](https://github.com/bol-edu/caravel-soc_fpga/blob/main/rtl/soc/housekeeping.v):58  
* [caravel-soc_fpga/rtl/soc/mprj_io.v](https://github.com/bol-edu/caravel-soc_fpga/blob/main/rtl/soc/mprj_io.v):58  
* [caravel-soc_fpga/rtl/soc/housekeeping_spi.v](https://github.com/bol-edu/caravel-soc_fpga/blob/main/rtl/soc/housekeeping_spi.v):16  
* [caravel-soc_fpga/rtl/soc/mgmt_core_wrapper.v](https://github.com/bol-edu/caravel-soc_fpga/blob/main/rtl/soc/mgmt_core_wrapper.v):30  
* [caravel-soc_fpga/rtl/soc/gpio_defaults_block.v](https://github.com/bol-edu/caravel-soc_fpga/blob/main/rtl/soc/gpio_defaults_block.v):58
    
**redeclaration of ansi port (checked by Xilinx xvlog)**  
* [caravel-soc_fpga/rtl/user/user_proj_example.counter.v](https://github.com/bol-edu/caravel-soc_fpga/blob/main/rtl/user/user_proj_example.counter.v)  
* [caravel-soc_fpga/rtl/user/user_proj_example.gcd.v](https://github.com/bol-edu/caravel-soc_fpga/blob/main/rtl/user/user_proj_example.gcd.v)  
* [caravel-soc_fpga/rtl/soc/gpio_control_block.v](https://github.com/bol-edu/caravel-soc_fpga/blob/main/rtl/soc/gpio_control_block.v)  
* [caravel-soc_fpga/rtl/soc/gpio_defaults_block.v](https://github.com/bol-edu/caravel-soc_fpga/blob/main/rtl/soc/gpio_defaults_block.v)    
* [caravel-soc_fpga/rtl/soc/housekeeping.v](https://github.com/bol-edu/caravel-soc_fpga/blob/main/rtl/soc/housekeeping.v)
   
**comment out `timescale 1 ns / 1 ps (checked by Xilinx xelab)**  
* [caravel-soc_fpga/testbench/counter_la_tb.v](https://github.com/bol-edu/caravel-soc_fpga/blob/main/testbench/counter_la/counter_la_tb.v)  
* [caravel-soc_fpga/testbench/counter_wb_tb.v](https://github.com/bol-edu/caravel-soc_fpga/blob/main/testbench/counter_wb/counter_wb_tb.v)  
* [caravel-soc_fpga/testbench/gcd_la_tb.v](https://github.com/bol-edu/caravel-soc_fpga/blob/main/testbench/gcd_la/gcd_la_tb.v)  
* [caravel-soc_fpga/vip/tbuart.v](https://github.com/bol-edu/caravel-soc_fpga/blob/main/vip/tbuart.v)  
* [caravel-soc_fpga/vip/spiflash.v](https://github.com/bol-edu/caravel-soc_fpga/blob/main/vip/spiflash.v)
   
### Revision from Efabless Caravel harness SoC to Caravel SoC (No PDK)
In first revision stage, we bypassed modules which have SKY PDK dependencies and removed package-facing pins like VDD, VS, VCC. But we had kept the same logic behavior of whole Caravel.  

* [caravel-soc_fpga/rtl/soc-efabless/caravel.v](https://github.com/bol-edu/caravel-soc_fpga/blob/main/rtl/soc-efabless/caravel.v) was modified to [caravel-soc_fpga/rtl/soc/caravel.v](https://github.com/bol-edu/caravel-soc_fpga/blob/main/rtl/soc/caravel.v)  
* [caravel-soc_fpga/rtl/soc-efabless/chip_io.v](https://github.com/bol-edu/caravel-soc_fpga/blob/main/rtl/soc-efabless/chip_io.v) was modified to [caravel-soc_fpga/rtl/soc/chip_io.v](https://github.com/bol-edu/caravel-soc_fpga/blob/main/rtl/soc/chip_io.v)  
* [caravel-soc_fpga/rtl/soc-efabless/gpio_control_block.v](https://github.com/bol-edu/caravel-soc_fpga/blob/main/rtl/soc-efabless/gpio_control_block.v) was modified to [caravel-soc_fpga/rtl/soc/gpio_control_block.v](https://github.com/bol-edu/caravel-soc_fpga/blob/main/rtl/soc/gpio_control_block.v)  
* [caravel-soc_fpga/rtl/soc-efabless/gpio_defaults_block.v](https://github.com/bol-edu/caravel-soc_fpga/blob/main/rtl/soc-efabless/gpio_defaults_block.v) was modified to [caravel-soc_fpga/rtl/soc/gpio_defaults_block.v](https://github.com/bol-edu/caravel-soc_fpga/blob/main/rtl/soc/gpio_defaults_block.v)  
* [caravel-soc_fpga/rtl/soc-efabless/mprj_io.v](https://github.com/bol-edu/caravel-soc_fpga/blob/main/rtl/soc-efabless/mprj_io.v) was modified to [caravel-soc_fpga/rtl/soc/mprj_io.v](https://github.com/bol-edu/caravel-soc_fpga/blob/main/rtl/soc/mprj_io.v)  
* [caravel-soc_fpga/rtl/soc-efabless/housekeeping.v](https://github.com/bol-edu/caravel-soc_fpga/blob/main/rtl/soc-efabless/housekeeping.v) was modified to [caravel-soc_fpga/rtl/soc/housekeeping.v](https://github.com/bol-edu/caravel-soc_fpga/blob/main/rtl/soc/housekeeping.v)   
   
### Behavior spiflash.v to RTL spiflash.v and Behavior bram.v
The original Efabless Caravel harness used behavior [caravel-soc_fpga/vip/spiflash.v](https://github.com/bol-edu/caravel-soc_fpga/blob/main/vip/spiflash.v). We rewrote to RTL [caravel-soc_fpga/rtl/soc/spiflash.v](https://github.com/bol-edu/caravel-soc_fpga/blob/main/rtl/soc/spiflash.v) (FPGA synthesizable) and behavior [caravel-soc_fpga/vip/bram.v](https://github.com/bol-edu/caravel-soc_fpga/blob/main/vip/bram.v) (needs FPGA BRAM porting). The behavior bram module was also be instantiated in corresponded Caravel testbench.

### Example of Running Xilinx xvlog/xelab/xsim Command-line
```console
xvlog -d FUNCTIONAL -d SIM -d DUNIT_DELAY=#1 -d USE_POWER_PINS -f ./include.rtl.list.xsim $design_tb.v
xelab -top $design_tb -snapshot $design_tb_elab
xsim $design_tb_elab -R
```
## Appendix: Boledu Vitis Machine
Boledu OnlineFPGA ([application form](https://docs.google.com/forms/d/e/1FAIpQLScZQZXyrtWZiT6r6_HkJhWV-VPNmRv6qAE5ehtmu2Qz-71V4w/viewform)) provide presetup Xilinx Vitis Ubuntu machines. The follow Vitis machine user flow demonstrates the usages of Xilinx Vitis .

* OnlineFPGA login
```
######################################################
#                                                    #
#       Welcome to BoLedu's OnlineFPGA service       #
#        Renting OnlineFPGA from service menu        #
#                                                    #
#       Shutdown: TPE(UTC+8) 6:00am to 7:00am        #
#                                                    #
#            email: onlinefpga@gmail.com             #
#                                                    #
######################################################

[1] login evaluation
[0] exit OnlineFPGA service

please enter your option:
>> 1
please enter your register email:
>> caravel.user@gmail.com
please enter your password:
>> *****
```
* OnlineFPGA rent - Vitis Machine
```
[1] list all online device boards
[2] rent a specified device board
[3] return your rented device board
[0] exit OnlineFPGA service

please enter your option:
>> 1
all online device boards
{'device': 'pynq_01',  'status': 'available'}
{'device': 'pynq_02',  'status': 'unknown'}
{'device': 'pynq_03',  'status': 'available'}
{'device': 'pynq_04',  'status': 'available'}
{'device': 'pynq_05',  'status': 'available'}
{'device': 'pynq_06',  'status': 'available'}
{'device': 'pynq_07',  'status': 'available'}
{'device': 'pynq_09',  'status': 'available'}
{'device': 'pynq_11',  'status': 'available'}
{'device': 'pynq_12',  'status': 'available'}
{'device': 'pynq_13',  'status': 'available'}
{'device': 'pynq_14',  'status': 'available'}
{'device': 'pynq_15',  'status': 'available'}
{'device': 'pynq_16',  'status': 'unknown'}
{'device': 'pynq_17',  'status': 'available'}
{'device': 'pynq_18',  'status': 'available'}
{'device': 'u50_01',   'status': 'available', 'user': 0, 'queue_user': 0, 'u50_board': 1, 'tenant': 9}
{'device': 'u50_02',   'status': 'available', 'user': 0, 'queue_user': 0, 'u50_board': 1, 'tenant': 8}
{'device': 'u50_03',   'status': 'available', 'user': 0, 'queue_user': 0, 'u50_board': 1, 'tenant': 8}
{'device': 'u50_04',   'status': 'available', 'user': 0, 'queue_user': 0, 'u50_board': 1, 'tenant': 7}

[1] list all online device boards
[2] rent a specified device board
[3] return your rented device board
[0] exit OnlineFPGA service

please enter your option:
>> 2

[1] pynq
[2] u50

please enter your option:
>> 2

[1] vitis tool

please enter your option:
>> 1
all online device boards
{'device': 'u50_01',   'status': 'available', 'user': 0, 'queue_user': 0, 'u50_board': 1, 'tenant': 9}
{'device': 'u50_02',   'status': 'available', 'user': 0, 'queue_user': 0, 'u50_board': 1, 'tenant': 8}
{'device': 'u50_03',   'status': 'available', 'user': 0, 'queue_user': 0, 'u50_board': 1, 'tenant': 8}
{'device': 'u50_04',   'status': 'available', 'user': 0, 'queue_user': 0, 'u50_board': 1, 'tenant': 7}

please enter u50 device name which you want to rent:
>> u50_02
device u50_02 is available
do you want to rent this device? (y/n)
>> y
user caravel.user@gmail.com rented device u50_02 successfully
ssh ip port is 218.103.115.199:1200, ssh username/passwd is 02.pdhkcv and timeup at 05/25/2023 11:40:30
```

* We use [MobaXterm](https://mobaxterm.mobatek.net/download.html) to SSH host `218.103.115.199`, port `1200`, username `02.pdhkcv` and passwd `02.pdhkcv`.
![boledu vitis01](https://github.com/bol-edu/caravel-soc_fpga/assets/98332019/eeeb0e4b-d0d8-4e83-b6a9-96fda7fbb116)

* The Boledu OnlineFPGA users don't have sudo permission but they can run all commands used in Caravel SoC FPGA project. We demonstrate usages of `run_xsim`, `gtkwave` and `run_vivado`.
```console
02.pdhkcv@HLS02:~$ git clone https://github.com/bol-edu/caravel-soc_fpga
Cloning into 'caravel-soc_fpga'...
remote: Enumerating objects: 1386, done.
remote: Counting objects: 100% (348/348), done.
remote: Compressing objects: 100% (223/223), done.
remote: Total 1386 (delta 242), reused 175 (delta 125), pack-reused 1038
Receiving objects: 100% (1386/1386), 13.37 MiB | 20.34 MiB/s, done.
Resolving deltas: 100% (758/758), done.
Updating files: 100% (570/570), done.
02.pdhkcv@HLS02:~$ cd caravel-soc_fpga/testbench/counter_la/
~/caravel-soc_fpga/testbench/counter_la ~
02.pdhkcv@HLS02:~/caravel-soc_fpga/testbench/counter_la$ ./run_xsim
...
... boledu skip lot of simulation info...
...
LA Test 1 started
LA Test 2 passed
$finish called at time : 974137500 ps : File "/mnt/HLSNAS/02.pdhkcv/caravel-soc_fpga/testbench/counter_la/counter_la_tb.v" Line 164
exit
INFO: [Common 17-206] Exiting xsim at Thu May 25 10:13:18 2023...
02.pdhkcv@HLS02:~/caravel-soc_fpga/testbench/counter_la$ gtkwave waveform.gtkw
GTKWave Analyzer v3.3.103 (w)1999-2019 BSI
[0] start time.
[974125000] end time.
WM Destroy
```
![boledu vitis02](https://github.com/bol-edu/caravel-soc_fpga/assets/98332019/8fb24fca-98b0-4a6c-8e4a-7fd5a9aeaae2)
```console
02.pdhkcv@HLS02:~/caravel-soc_fpga/testbench/counter_la$ cd ~/caravel-soc_fpga/vivado/
~/caravel-soc_fpga/vivado ~/caravel-soc_fpga/testbench/counter_la
02.pdhkcv@HLS02:~/caravel-soc_fpga/vivado$ ./run_vivado
start vivado project
****** Vivado v2022.1 (64-bit)
  **** SW Build 3526262 on Mon Apr 18 15:47:01 MDT 2022
  **** IP Build 3524634 on Mon Apr 18 20:55:01 MDT 2022
    ** Copyright 1986-2022 Xilinx, Inc. All Rights Reserved.
...
... boledu skip lot of simulation info...
...
open_run: Time (s): cpu = 00:00:11 ; elapsed = 00:00:10 . Memory (MB): peak = 3469.965 ; gain = 253.809 ; free physical = 38116 ; free virtual = 75592
# report_timing_summary -file timing_report.log
INFO: [Timing 38-91] UpdateTimingParams: Speed grade: -1, Delay Type: min_max.
INFO: [Timing 38-191] Multithreading enabled for timing update using a maximum of 8 CPUs
# exit
INFO: [Common 17-206] Exiting Vivado at Thu May 25 10:23:31 2023...
======================================================================
                           vivado complete
======================================================================
02.pdhkcv@HLS02:~/caravel-soc_fpga/vivado$
```

## Appendix: AWS EC2 Vitis Subscription
* [Register your AWS account](https://portal.aws.amazon.com/billing/signup#/start/email)
* [Sign in AWS console](https://aws.amazon.com/console)
* The Xilinx Vitis 2022.1 can be found in these regions: us-east-1, us-east-2, us-west-1 and us-west-2
* [We use AWS EC2 us-east-1](https://us-east-1.console.aws.amazon.com/ec2)
* Images -> AMI Catalog -> Search 'vitis 2022.1' in AMIs
![aws-001](https://github.com/bol-edu/caravel-soc_fpga/assets/98332019/6672497a-7fc0-4ad5-b29b-18923b935b23)

* Select
![aws-002](https://github.com/bol-edu/caravel-soc_fpga/assets/98332019/e69795ed-2003-4829-9134-72f1f6f0c715)

* Continue
![aws-003](https://github.com/bol-edu/caravel-soc_fpga/assets/98332019/6fb53f21-bdc3-4c02-b332-46d06249134c)

* Launch Instance with AMI
![aws-004](https://github.com/bol-edu/caravel-soc_fpga/assets/98332019/ce1f2df6-f921-42a7-9d2c-85e05534aacb)

* Name -> vitis.server
![aws-004 x](https://github.com/bol-edu/caravel-soc_fpga/assets/98332019/9d875514-103a-479a-b942-91644e22bfb9)

* Change to m5.2xlarge Instance type and Create new key pair
![aws-005](https://github.com/bol-edu/caravel-soc_fpga/assets/98332019/8d535f05-5bb4-48aa-8e16-d89dedb48649)

* Create key pair name 'vitis.rsa.key' and key pair file is needed to download to local
![aws-006](https://github.com/bol-edu/caravel-soc_fpga/assets/98332019/d0b7eb39-f012-4dee-90e0-bc7ac8f3549b)

* Create security group -> Confirm Summary (Vitis 2022.1 AMI, m5.2xlarge Instance, New security group and 220GiB volume) -> Launch instance
![aws-007](https://github.com/bol-edu/caravel-soc_fpga/assets/98332019/70a2e182-e2d4-474b-9230-4fab6db877ee)

* The EC2 instance is running
![aws-008](https://github.com/bol-edu/caravel-soc_fpga/assets/98332019/fe838abe-0dcc-437a-9a05-727b1f3e3d6b)

* Click EC2 instance i-00aff7b2ccd9cb2c1 (vitis.server) to find Public IPv4 address '18.207.96.216'
![aws-009](https://github.com/bol-edu/caravel-soc_fpga/assets/98332019/93540274-fb62-447e-afa6-71bb3de0908b)

* We use [MobaXterm](https://mobaxterm.mobatek.net/download.html) to connect EC2 Ubuntu vitis.server  
  * host: 18.207.96.216 (use your found host)  
  * username: ubuntu  
  * private key: vitis.rsa.key.pem (use downloaded key pair file)
![aws-010](https://github.com/bol-edu/caravel-soc_fpga/assets/98332019/a41959b6-ac2c-415b-ad35-34aecc5e0eea)

* Login EC2 Ubuntu vitis.server
![aws-011](https://github.com/bol-edu/caravel-soc_fpga/assets/98332019/1e558738-1752-4c1e-9904-fdf5f8f958f6)

* While finishing usage of vitis.server, remember go to EC2 console and select Running instance i-00aff7b2ccd9cb2c1 -> Instance state -> Terminate instance
![aws-012](https://github.com/bol-edu/caravel-soc_fpga/assets/98332019/d55f3e86-0ac5-4aa3-84da-e68564f6f749)

## Appendix: Reference Documents
### Caravel Offical
* [Quickstart of How to Use Caravel User Project](https://github.com/efabless/caravel_user_project/blob/main/docs/source/index.rst#section-quickstart)
* [Caravel User Project](https://caravel-user-project.readthedocs.io/en/latest/#caravel-user-project)
* [Efabless Caravel “harness” SoC](https://caravel-harness.readthedocs.io/en/latest/#efabless-caravel-harness-soc)
* [Efabless Caravel Specification](https://github.com/efabless/caravel/tree/main/docs/pdf)

### Xilinx Vitis/Vivado Lab
* [Boledu course-lab_1](https://github.com/bol-edu/course-lab_1)
* [Boledu course-lab_2](https://github.com/bol-edu/course-lab_2)
