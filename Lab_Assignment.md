# EX1: Testing the platform on Vitis  
After setting up the vitis enviroment and creating an helloworld project you can start to modify the program for testing purposes.  
Under the `<name>_system > src` folder you can find the `helloworld.c` program whereas under the `<xsa_file_name> > hw > drivers > <ip_name> > src` you can find
the .h file that defines some useful read and write macros you should use and the address offsets.  
The base address can be found under `<xsa_file_name> > hw > <xsa_file_name>.xsa`, just scroll to the bottom of the list and you should see the name of your ip with the base address.  
At this point you have everything you need to create a test application to test the functionality of the hardware.  

When you're done creating the application create a new command shell console:
- connection type: serial port
- select `new...` under connection name and select the right usb port (the one connected to the pynq board)
  - if you don't see any usb ports it means that you need to change the permission of the usb device found under the `/dev` directory
- select ok

Make sure you have turned on the board and that you have unplugged the boot jumper from the board, this will make the board ready for flashing.  
At this point you can build the application and by clicking on the play icon.
# EX2: Creating the Driver  
In this exercise you are required to write a driver starting from a template that can be found under the `Templates` directory. 
  
In order to communicate with a memory mapped device it's not sufficient to access memory using pointers, if you do this using the base address the driver with segfault.  
This is because the memory of the device is CPU-memory whereas the driver is in a limited part of kernel memory.  
  
In order to link a piece of kernel memory to CPU-memory, use the following function:  
```console
void __iomem * ioremap(BASE_ADDRESS, SIZE)
```
`BASE_ADDRESS` of type `phys_addr_t` being the base address of the core and `SIZE` of type `size_t` being the memory depth of the core in bytes (4 bytes for each register in the axi).  
This function returns a pointer that is liked to the base address of the core and can be used to read and write to the registers.  
  
Reading and writing to the registers needs to be an atomic operation, meaning that no interrupts can interrupt the reading or writing process.  
In order to do so we need to use some specific macros: 
```console
void writel(VALUE, ADDRESS)
```
```console
u32 readl(ADDRESS)
```
Where `ADDRESS` is of type `void __iomem *` and `VALUE` is of type `u32`.  
All of these macros are defined in the `Linux/io.h` header file. 
<br>
If you are unsure about the base address, you can find it in the Device Tree recipe for the core that can be located in `PROGETTO/components/plnx_workspace/device-tree/device-tree/pl.dtsi`. This file is created after running `petalinux-build`, the base address is under the `reg` field.  
<br>
In the `solutions` directory you can find our implementation. It's important to note that our implementation could not be working with your core because of different base addresses.  
NOTE: after booting onto the board the driver can be found in `/lib/modules/<directory>/extra/` where `<directory>` is the only directory present in that path, the name is not known.  

# EX3: Creating the Test Application
In this exercise you are required to write a test application starting from a template that can be found under the `Templates` directory.  
The application should do some cryptography operations on some data. The actual functionality is up to you.  
A good example is an application that takes a .txt file and either encrypts or decrypts it using a key.  
  
NOTE: after booting onto the board the application can be found in `/usr/bin`.
