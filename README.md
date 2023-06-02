# embedded-LINUX-crypto-core
## introduction
This repo has been created by group #5 of the OS course.  
In this repo you can find everything needed to complete the laboratory experience, including the solutions.  
<br>
# GETTING STARTED
## pre-requisites
In order to insure compatibility the following steps are required:  
- if you want to build your own BSP check the vivado version used by the [pynq project](https://github.com/xilinx/pynq). At the time of writing the compatible version is the 2022.1 but this **could change in the future**. The pre-packaged BSP supports version 2022.1 
- the same version should be used for the petalinux software, note that [petalinux only supports some linux distros](https://docs.xilinx.com/r/2022.1-English/ug1144-petalinux-tools-reference-guide/Installation-Requirements "petalinux 2022.1 installation requirements"). For petalinux 2022.1 ubuntu 20.04 was used.  
  - make sure you read the petalinux installation requirements carefully, expecially the free space requirement and the `bin/sh` shell requirement.  
  - make sure to install the required packages referenced in the excel file located in the [petalinux release notes webpage](https://support.xilinx.com/s/article/000033799?language=en_US) **for your installed version**.  
<br>

## Installing the software
- download and install the required version of vitis and vivado from the [xilinx website](https://www.xilinx.com/support/download/index.html/content/xilinx/en/downloadNav/vivado-design-tools/2022-1.html).
- download and install the required version of petalinux from the [xilinx website](https://www.xilinx.com/support/download/index.html/content/xilinx/en/downloadNav/embedded-design-tools/2022-1.html).  
<br>

## Laboratory experience flow

- you will first need to either build your own or download the BSP
- once you have the BSP you will need to create the custom hardware platform in vivado using the provided VHDL files
- after exporting the hardware platform, you will need to test the hardware in baremetal using vitis
- you will then need to create a new petalinux project using the bsp and the hardware platform
- after that you will build the kernel
- once you are done building you will need to package the boot components and load them into an sd card
- after booting from the sd card and making sure that the linux image works correctly, you will be asked to create a custom linux driver
- you will then be walked through the steps required to load the driver and test it using a custom user application
<br>

# The pynq-z2 BSP
In order to create a petalinux project we first need a BSP.  
The BSP for the pynq board is not provided by the board maker but you can create it using petalinux from the [pynq project](https://github.com/xilinx/pynq).  
In order to save time, a version of the bsp can be downloaded [here](https://mega.nz/file/NHNHzTYB#UCP6dy67P80xzV5r_mvvZvwBLSUyGVFZmOuWuZBe2bQ). This version only works with petalinux 2022.1.  

## Creating the BSP from scratch  
- open a new terminal on your desktop
- install the git application 
  ```console
  sudo apt install git
  ```
- clone the pynq project repository to your local machine 
  ```console
  git clone https://github.com/Xilinx/PYNQ.git
  ```
- move to the `boards/pynq_z2/base` folder in the newly created PYNQ folder.  
  ```console
  cd PYNQ/boards/Pynq-Z2/base/
  ```
- source the vivado enviroment variables. Replace `<your vivado install dir>` with the installion directory of the vivado tool  
  ```console
  source <your vivado install dir>/settings64.sh
  ```
- run the `make` command  
  ```console
  make
  ```
  - please note that this may take a **very long time**, just let it run.
- once the program finishes you can close the terminal.
- open a new terminal in the desktop
- source the petalinux enviroment variables. Replace `<your petalinux install dir>` with the installion directory of the petalinux tool
  ```console
  source <your petalinux install dir>/settings.sh
  ```
- create a new project from template
  ```console 
  petalinux-create --type project --template zynq --name PYNQ_Z2
  ```
- move to the newly created folder
  ```console
  cd PYNQ_Z2/
  ```
- run the `petalinux-config` command specifing the hardware platform
  ```console
  petalinux-config --get-hw-description ../PYNQ/boards/Pynq-Z2/base/base.xsa
  ```
  - this will open a configuration tab. You don't need to modify anything at this stage so you can close and save by navigating to the `<exit>` option.
- run the `petalinux-package` command to package the BSP
  ```console
  petalinux-package --bsp -p ../PYNQ_Z2 --output pynq_z2_bsp
  ```
- you have successfully created your bsp which can be found in the project directory.  

<br>

# creating the hardware project
In the vhdl folder you can find everything you need to use the crypto core. There you will also find a readme file with the steps needed to instantiate all the required IPs.  
