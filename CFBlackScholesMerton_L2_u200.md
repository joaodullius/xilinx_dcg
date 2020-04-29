## Run Black-Scholes-Merton Closed Form Demonstration (Layer 2) - U200


### Step 1 :
Setup the build environment using the Vitis and XRT scripts:
````
source /opt/xilinx/Vitis/2019.2/settings64.sh
source /opt/xilinx/xrt/setup.sh
 ````
 
```
cd /data/Vitis_Libraries/quantitative_finance/L2/tests/CFBlackScholesMerton
 ```
 
 ### Step 2 : Software Emulation

Call the Makefile passing in the intended target and device. The Makefile supports software emulation, hardware emulation and hardware targets ('sw_emu', 'hw_emu' and 'hw', respectively). For example to build and run the test application:
```
make check TARGET=sw_emu DEVICE=xilinx_u200_xdma_201830_2
```
        
For all Makefile targets, the host application and xclbin are delivered to named folders depending on the target and part selected. For example, the command above will produce:
```
./bin_xilinx_u200_xdma_201830_2/bs_test.exe
./xclbin_xilinx_u200_xdma_201830_2_sw_emu/bs_kernel.xclbin
```

To run the application again in Software Emulation, run the follow commands:
Note that 16384 is the number of option to be calculated.
```
export XCL_EMULATION_MODE=sw_emu
./bin_xilinx_u200_xdma_201830_2/bsm_test.exe ./xclbin_xilinx_u200_xdma_201830_2_sw_emu/bsm_kernel.xclbin 16384
```

Expected results:
```
*************
BSM Demo v1.0
*************

Generating randomized data and reference results...
Connecting to device and loading kernel...
Found Platform
Platform Name: Xilinx
INFO: Importing ./xclbin_xilinx_u200_xdma_201830_2_sw_emu/bsm_kernel.xclbin
Loading: './xclbin_xilinx_u200_xdma_201830_2_sw_emu/bsm_kernel.xclbin'
Allocating buffers...
Launching kernel...
  Duration returned by profile API is 14724.9 ms ****
Kernel done!
Comparing results...
Processed 16384 call options:
Throughput = 0.00111267 Mega options/sec

  Largest host-kernel price difference = -5.07147e-05
  Largest host-kernel delta difference = 3.3043e-07
  Largest host-kernel gamma difference = 3.56924e-08
  Largest host-kernel vega difference  = 3.19532e-07
  Largest host-kernel theta difference = -3.94425e-08
  Largest host-kernel rho difference   = -1.11218e-06

  ```
Observe that for 16384 call options it took 14,74 seconds in software emulation.

### Step 3: Run in Hardware
Now, let´s run Makefile for the Hardware mode.
This will take a while. In the Nimbix box it took **4h 27m 40s**.
```
make all TARGET=hw DEVICE=xilinx_u200_xdma_201830_2
```

Change to a Nimbix instance with U200 Alveo board and run:
```
cd /data/Vitis_Libraries/quantitative_finance/L2/tests/CFBlackScholes
unset XCL_EMULATION_MODE
./bin_xilinx_u200_xdma_201830_2/bsm_test.exe ./xclbin_xilinx_u200_xdma_201830_2_hw/bsm_kernel.xclbin 16384
```

Expected results:
```

*************
BSM Demo v1.0
*************

Generating randomized data and reference results...
Connecting to device and loading kernel...
Found Platform
Platform Name: Xilinx
INFO: Importing ./xclbin_xilinx_u200_xdma_201830_2_hw/bsm_kernel.xclbin
Loading: './xclbin_xilinx_u200_xdma_201830_2_hw/bsm_kernel.xclbin'
Allocating buffers...
Launching kernel...
  Duration returned by profile API is 0.386809 ms ****
Kernel done!
Comparing results...
Processed 16384 call options:
Throughput = 42.3568 Mega options/sec

  Largest host-kernel price difference = -5.07147e-05
  Largest host-kernel delta difference = 3.3043e-07
  Largest host-kernel gamma difference = 3.56924e-08
  Largest host-kernel vega difference  = 3.19532e-07
  Largest host-kernel theta difference = -3.94425e-08
  Largest host-kernel rho difference   = -1.11218e-06
```

### Software Emulation vs Hardware Execution
Observe that Software Emulation took **14.7** seconds versus 0.38 microseconds in the Hardware.
The harware ran at a Throuput of **42.3568 Mega call options calculations per second.**

We can run Hardware Execution with 4194304 options to exercise full DDR bandwidth:
```
unset XCL_EMULATION_MODE
./bin_xilinx_u200_xdma_201830_2/bsm_test.exe ./xclbin_xilinx_u200_xdma_201830_2_hw/bsm_kernel.xclbin 4194304

*************
BSM Demo v1.0
*************

Generating randomized data and reference results...
Connecting to device and loading kernel...
Found Platform
Platform Name: Xilinx
INFO: Importing ./xclbin_xilinx_u200_xdma_201830_2_hw/bsm_kernel.xclbin
Loading: './xclbin_xilinx_u200_xdma_201830_2_hw/bsm_kernel.xclbin'
Allocating buffers...
Launching kernel...
  Duration returned by profile API is 14.6305 ms ****
Kernel done!
Comparing results...
Processed 4194304 call options:
Throughput = 286.682 Mega options/sec

  Largest host-kernel price difference = -8.5042e-05
  Largest host-kernel delta difference = 5.64083e-07
  Largest host-kernel gamma difference = -2.13355e-07
  Largest host-kernel vega difference  = 5.07098e-07
  Largest host-kernel theta difference = -4.70394e-08
  Largest host-kernel rho difference   = 1.65554e-06

```

Observe that it took **14.63ms** and a achieved a Throubput of **286.682 Mega call options/sec**

## Navigation
- Next: [Black-Scholes-Merton Closed Form Demonstration (Layer 3) - U200](CFBlackScholesMerton_L3_u200.md)
- Index: [Xilinx´s Quantitative Finances Examples / Walkthrough](quantitative_finance.md)

## Sources
This document is based and extends the following documents:
- https://github.com/Xilinx/Vitis_Libraries/tree/master/quantitative_finance
