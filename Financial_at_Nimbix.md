## Clone Vitis Libraries fro GitHub

```
git clone https://github.com/Xilinx/Vitis_Libraries.git /data/Vitis_Libraries
```

## Run Black-Scholes Closed Form Demonstration (Layer 2) 


### Step 1 :
Setup the build environment using the Vitis and XRT scripts:
````
        source /opt/xilinx/Vitis/2019.2/settings64.sh
        source /opt/xilinx/xrt/setup.sh
 ````
 
 ### Step 2 :
 ```
  cd /data/Vitis_Libraries/quantitative_finance/L2/tests/CFBlackScholes
  ```
 #### Software Emulation
Call the Makefile passing in the intended target and device. The Makefile supports software emulation, hardware emulation and hardware targets ('sw_emu', 'hw_emu' and 'hw', respectively). For example to build and run the test application:

        make check TARGET=sw_emu DEVICE=xilinx_u250_xdma_201830_2
        
For all Makefile targets, the host application and xclbin are delivered to named folders depending on the target and part selected. For example, the command above will produce:

        ./bin_xilinx_u250_xdma_201830_2/bs_test.exe
        ./xclbin_xilinx_u250_xdma_201830_2_sw_emu/bs_kernel.xclbin

To run the application again in Software Emulation, run the follow commands:
Note that 16384 is the number of option to be calculated, and needs to be in power of two (need to confirm this)

```
export XCL_EMULATION_MODE=sw_emu
./bin_xilinx_u250_xdma_201830_2/bs_test.exe ./xclbin_xilinx_u250_xdma_201830_2_sw_emu/bs_kernel.xclbin 16384
```

#### Run in Hardware
Now, let´s run Makefile for the Hardware mode. This will take a while:
```
make all TARGET=hw DEVICE=xilinx_u250_xdma_201830_2
```

The hardware emulation can be run in a similar way, but a smaller number of parameters should be used as an RTL simulation is used under-the-hood:

        export XCL_EMULATION_MODE=hw_emu
        ./bin_xilinx_u250_xdma_201830_2/bs_test.exe ./xclbin_xilinx_u250_xdma_201830_2_hw_emu/bs_kernel.xclbin 4096
Assuming an Alveo U250 card with the XRT configured the hardware build is run in the same way. Here a much large number of parameters should be used to fully exercise the DDR bandwidth:

        unset XCL_EMULATION_MODE
        ./bin_xilinx_u250_xdma_201830_2/bs_test.exe ./xclbin_xilinx_u250_xdma_201830_2_hw/bs_kernel.xclbin 4194304
