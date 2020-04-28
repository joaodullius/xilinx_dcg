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

        make check TARGET=sw_emu DEVICE=xilinx_u200_xdma_201830_2
        
For all Makefile targets, the host application and xclbin are delivered to named folders depending on the target and part selected. For example, the command above will produce:

        ./bin_xilinx_u200_xdma_201830_2/bs_test.exe
        ./xclbin_xilinx_u200_xdma_201830_2_sw_emu/bs_kernel.xclbin

To run the application again in Software Emulation, run the follow commands:
Note that 16384 is the number of option to be calculated, and needs to be in power of two (need to confirm this)

```
export XCL_EMULATION_MODE=sw_emu
./bin_xilinx_u200_xdma_201830_2/bs_test.exe ./xclbin_xilinx_u200_xdma_201830_2_sw_emu/bs_kernel.xclbin 16384
```

Expected results:
```
************
BS Demo v1.0
************

Generating randomized data and reference results...
Connecting to device and loading kernel...
CRITICAL WARNING: [HW-EM 08-0] Unable to find emconfig.json. Using default device "xilinx:pcie-hw-em:7v3:1.0"
Found Platform
Platform Name: Xilinx
INFO: Importing ./xclbin_xilinx_u200_xdma_201830_2_sw_emu/bs_kernel.xclbin
Loading: './xclbin_xilinx_u200_xdma_201830_2_sw_emu/bs_kernel.xclbin'
Allocating buffers...
Launching kernel...
  Duration returned by profile API is 12352.2 ms ****
Kernel done!
Comparing results...
Processed 16384 call options:
Throughput = 0.00132641 Mega options/sec

  Largest host-kernel price difference = -6.19518e-05
  Largest host-kernel delta difference = 4.00283e-07
  Largest host-kernel gamma difference = -5.58116e-08
  Largest host-kernel vega difference  = -3.3779e-07
  Largest host-kernel theta difference = -3.1506e-08
  Largest host-kernel rho difference   = 1.08024e-06
  ```
Observe that for 16384 call options it took 12,35 seconds in software emulation.

#### Run in Hardware
Now, let´s run Makefile for the Hardware mode.
This will take a while. In the Nimbix box it took **4h 4m 15s**.
```
make all TARGET=hw DEVICE=xilinx_u200_xdma_201830_2
```

Change to a Nimbix instance with U200 Alveo board and run:
```
cd /data/Vitis_Libraries/quantitative_finance/L2/tests/CFBlackScholes
unset XCL_EMULATION_MODE
./bin_xilinx_u200_xdma_201830_2/bs_test.exe ./xclbin_xilinx_u200_xdma_201830_2_hw/bs_kernel.xclbin 16384
```

Expected results:
```
************
BS Demo v1.0
************

Generating randomized data and reference results...
Connecting to device and loading kernel...
Found Platform
Platform Name: Xilinx
INFO: Importing ./xclbin_xilinx_u200_xdma_201830_2_hw/bs_kernel.xclbin
Loading: './xclbin_xilinx_u200_xdma_201830_2_hw/bs_kernel.xclbin'
Allocating buffers...
Launching kernel...
  Duration returned by profile API is 0.302784 ms ****
Kernel done!
Comparing results...
Processed 16384 call options:
Throughput = 54.1112 Mega options/sec

  Largest host-kernel price difference = -6.19518e-05
  Largest host-kernel delta difference = 4.00283e-07
  Largest host-kernel gamma difference = -5.58116e-08
  Largest host-kernel vega difference  = -3.3779e-07
  Largest host-kernel theta difference = -3.1506e-08
  Largest host-kernel rho difference   = 1.08024e-06
```

#### Software Emulation vs Hardware Execution
Observe that Software Emulation took **12.35** seconds versus 0.30 microseconds in the Hardware. That´s **40.795x** performance.
The harware ran at a Throuput of **54.1112 Mega call options calculations per second.**

We can run Hardware Execution with 4194304 options to exercise full DDR bandwidth:
```
unset XCL_EMULATION_MODE
./bin_xilinx_u200_xdma_201830_2/bs_test.exe ./xclbin_xilinx_u200_xdma_201830_2_hw/bs_kernel.xclbin 4194304

************
BS Demo v1.0
************

Generating randomized data and reference results...
Connecting to device and loading kernel...
Found Platform
Platform Name: Xilinx
INFO: Importing ./xclbin_xilinx_u200_xdma_201830_2_hw/bs_kernel.xclbin
Loading: './xclbin_xilinx_u200_xdma_201830_2_hw/bs_kernel.xclbin'
Allocating buffers...
Launching kernel...
  Duration returned by profile API is 13.4278 ms ****
Kernel done!
Comparing results...
Processed 4194304 call options:
Throughput = 312.361 Mega options/sec

  Largest host-kernel price difference = -7.96791e-05
  Largest host-kernel delta difference = -5.72562e-07
  Largest host-kernel gamma difference = 1.29485e-07
  Largest host-kernel vega difference  = 5.49625e-07
  Largest host-kernel theta difference = -4.7764e-08
  Largest host-kernel rho difference   = -1.72667e-06
```

Observe that it took **13.42ms** and a achieved a Throubput of **312.361 Mega call options/sec**


## Sources
This document is based and extends the following documents:
- https://github.com/Xilinx/Vitis_Libraries/tree/master/quantitative_finance
