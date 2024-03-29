## Creating the IP
- go to `tools>create and package new ip`
- select next, select `create a new AXI4 peripheral`
- name it `crypto_core_IP`
  
  ![](https://github.com/Giuseppe-La-Capra/embedded-LINUX-crypto-core/blob/37948db52606f45b5e17723d4aa23545a7f579c7/ip/Screenshot%20from%202023-06-04%2000-59-53.png)
- when promped to specify the AXI proprieties, change the number of registers to 5
  
  ![](https://github.com/Giuseppe-La-Capra/embedded-LINUX-crypto-core/blob/7282e9dd73bcdb285fd12fccb9a02565a35a1cb0/ip/Screenshot%20from%202023-06-04%2001-00-10.png)
- at this point we normally select `edit ip` but since vivado has some problems with the ip packager it's safer to select `add ip to repository`
  
  ![](https://github.com/Giuseppe-La-Capra/embedded-LINUX-crypto-core/blob/2ae29179fbc2338c1a1c9f0e924a2947ae241cc3/ip/Screenshot%20from%202023-07-18%2017-51-17.png)
- go to `IP catalog`, open `User Repository > AXI Peripheral` and right click on the ip
  
  ![](https://github.com/Giuseppe-La-Capra/embedded-LINUX-crypto-core/blob/2ae29179fbc2338c1a1c9f0e924a2947ae241cc3/ip/Screenshot%20from%202023-07-18%2017-51-50.png)
- select `Edit in IP Packager` 
- right click on `design sources`, select `Add Sources` and add all files from the `vhdl/src/` folder as sources
  
  ![](https://github.com/Giuseppe-La-Capra/embedded-LINUX-crypto-core/blob/2ae29179fbc2338c1a1c9f0e924a2947ae241cc3/ip/Screenshot%20from%202023-07-18%2017-38-50.png)
  ![](https://github.com/Giuseppe-La-Capra/embedded-LINUX-crypto-core/blob/2ae29179fbc2338c1a1c9f0e924a2947ae241cc3/ip/Screenshot%20from%202023-06-04%2001-01-00.png)
- navigate to `File Groups`, right click on `VHDL Sinthesis` and select `Add Files`

  ![](https://github.com/Giuseppe-La-Capra/embedded-LINUX-crypto-core/blob/2d19bad0a49f139d057d9d5ee7a8a58d4310d15a/ip/Screenshot%20from%202023-07-18%2017-39-50.png)
- add all .coe files located in `vhdl/mem_init_files` making sure that the option to copy files is checked

  ![](https://github.com/Giuseppe-La-Capra/embedded-LINUX-crypto-core/blob/2d19bad0a49f139d057d9d5ee7a8a58d4310d15a/ip/Screenshot%20from%202023-06-04%2001-03-58.png)
- at this point you can follow steps 3 to 6 in the `readme.txt` located in the `vhdl/` folder to add all the required IPs making sure to select the .coe files you just copied
- once done you can go to `File Groups` and select `merge changes from file group wizard`

  ![](https://github.com/Giuseppe-La-Capra/embedded-LINUX-crypto-core/blob/2d19bad0a49f139d057d9d5ee7a8a58d4310d15a/ip/Screenshot%20from%202023-06-04%2001-01-32.png)
  - at this point you might need to change the `type` field under `ip file group proprieties` of the folder `VHDL Simulation` to `simulation` instead of `VHDL:simulation`

    ![](https://github.com/Giuseppe-La-Capra/embedded-LINUX-crypto-core/blob/291966de89f5ba5f61383092b6ec3028edc65840/ip/Screenshot%20from%202023-07-18%2018-42-48.png)
    
<br>
now you can modify the vhdl file of the ip to insert the core as a component:
<br>

![](https://github.com/Giuseppe-La-Capra/embedded-LINUX-crypto-core/blob/aa6f0f391c17ea67ab9e16d1b8039e1c6efc0466/ip/Screenshot%20from%202023-07-18%2018-52-41.png)

- add the component in the vhdl description

  ![](https://github.com/Giuseppe-La-Capra/embedded-LINUX-crypto-core/blob/9a56a82d7d5cf33b83bece7871807cd9cb4ce254/ip/Screenshot%20from%202023-06-02%2021-16-31.png)
- add the required signals

  ![](https://github.com/Giuseppe-La-Capra/embedded-LINUX-crypto-core/blob/9a56a82d7d5cf33b83bece7871807cd9cb4ce254/ip/Screenshot%20from%202023-06-02%2021-18-11.png)
- connect the signals to the output of the axi

  ![](https://github.com/Giuseppe-La-Capra/embedded-LINUX-crypto-core/blob/9a56a82d7d5cf33b83bece7871807cd9cb4ce254/ip/Screenshot%20from%202023-06-02%2021-27-15.png)
- connect signals to the component and to the registers

  ![](https://github.com/Giuseppe-La-Capra/embedded-LINUX-crypto-core/blob/9a56a82d7d5cf33b83bece7871807cd9cb4ce254/ip/Screenshot%20from%202023-06-02%2021-30-35.png)
- at this point you can save the changes by selecting `review and package > re-package IP`
<br>
You have successfully created the IP. This should also close the IP Packager window and return to the original Vivado project.

## Creating the Block Diagram
- select `create block diagram` and then select `next`
  
  ![](https://github.com/Giuseppe-La-Capra/embedded-LINUX-crypto-core/blob/2cf9373336a6c958b23efe3bc1afad7fa51b49ec/ip/Screenshot%20from%202023-07-18%2019-05-50.png)
- click on the `+` simbol to add the zynq7000 processing system by double clicking on it

  ![](https://github.com/Giuseppe-La-Capra/embedded-LINUX-crypto-core/blob/2cf9373336a6c958b23efe3bc1afad7fa51b49ec/ip/Screenshot%20from%202023-07-18%2019-06-14.png)
- click on `run block automation`, leave everything default and click `next`

  ![](https://github.com/Giuseppe-La-Capra/embedded-LINUX-crypto-core/blob/2cf9373336a6c958b23efe3bc1afad7fa51b49ec/ip/Screenshot%20from%202023-07-18%2019-07-08.png)
- add the `crypto_core_ip` the same way you added the zynq 7000 and click on `run connection automation`

  ![](https://github.com/Giuseppe-La-Capra/embedded-LINUX-crypto-core/blob/dfd8de012d409ffee206c68b925e58d6076ac7fd/ip/Screenshot%20from%202023-07-18%2019-14-13.png)
- now go back to sources and right click on `design_1.db`

  ![](https://github.com/Giuseppe-La-Capra/embedded-LINUX-crypto-core/blob/3d78bbe7c7bfa73b5463a1560f105a716493d4e0/ip/Screenshot%20from%202023-07-18%2019-16-45.png)
- select `create HDL wrapper`, leave everything as default and press `ok`
- when you are done you can generate the bitstream

  ![](https://github.com/Giuseppe-La-Capra/embedded-LINUX-crypto-core/blob/be2f9b2620849e0cf8537f198617fffed57ab451/ip/Screenshot%20from%202023-07-18%2019-19-55.png)
- then go to `file > export > export hardware`, make sure to include the bitstream and remember where you save the .xsa file because you will need it later

  ![](https://github.com/Giuseppe-La-Capra/embedded-LINUX-crypto-core/blob/be2f9b2620849e0cf8537f198617fffed57ab451/ip/Screenshot%20from%202023-07-18%2019-21-58.png)

You have successfully created the hardware platform.
