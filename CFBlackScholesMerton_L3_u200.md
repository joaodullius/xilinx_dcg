## Run Black-Scholes-Merton Closed Form Demonstration (Layer 3) - U200


### Step 1 :
Setup the build environment using the Vitis and XRT scripts:
```
source /opt/xilinx/Vitis/2019.2/settings64.sh
source /opt/xilinx/xrt/setup.sh
```
 
### Step 2 : Software Emulation Mode
  
#### Step 2.1:  Build the L3 Library
```
cd /data/Vitis_Libraries/quantitative_finance/L3/src
source env.sh or source env.csh
make
```

#### Step 2.2: Build the matching CFBlackScholesMerton Kernel (option)
**Attention:** You don´t need to run this step if you already ran the [Black-Scholes-Merton Closed Form Demonstration (Layer 2)](CFBlackScholesMerton_L2_u200.md) in sw_emu mode before.
```
cd /data/Vitis_Libraries/quantitative_finance/L2/tests/CFBlackScholesMerton
make xclbin TARGET=sw_emu DEVICE=xilinx_u200_xdma_201830_2
```

#### Step 2.3: Build host code & run executable
```
cd L3/tests/CFBlackScholesMerton
make run TARGET=sw_emu DEVICE=xilinx_u200_xdma_201830_2
```
**Attention:** If you get a XILINX_XCL2_DIR error when running make run, you need to source the env.sh from Step 2.1.

  
  
  
  


Expected results:
```
xf_fintech_cf_black_scholes_merton.o
g++ -std=c++11 -g -O3 -Wall -Wno-unknown-pragmas -c -DDEVICE_PART=u200 -I/data/Vitis_Libraries/quantitative_finance/L3/include -I/data/Vitis_Libraries/quantitative_finance/L2/include -I/data/Vitis_Libraries/quantitative_finance/ext/xcl2 -I/opt/xilinx/xrt/include -o ./output/xf_fintech_cf_black_scholes_merton.o -c xf_fintech_cf_black_scholes_merton.cpp
g++ -o ./output/cfBSMEngine_example.exe ./output/xf_fintech_cf_black_scholes_merton.o -lpthread -lstdc++ -lxilinxfintech -lxilinxopencl -L/data/Vitis_Libraries/quantitative_finance/L3/src/output -L/opt/xilinx/xrt/lib
Found Platform
Platform Name: Xilinx
[XLNX] Found 1 matching devices
INFO: Importing bsm_kernel.xclbin
Loading: 'bsm_kernel.xclbin'
[XLNX] Binary Import Time = 11385 microseconds
[XLNX] Device Programming Time = 391961 microseconds
[XLNX] +-------+----------+----------+----------+----------+----------+----------+
[XLNX] | Index |  Price   |  Delta   |  Gamma   |   Vega   |  Theta   |   Rho    |
[XLNX] +-------+----------+----------+----------+----------+----------+----------+
[XLNX] |     0 |  0.00000 |  0.00000 |  0.00000 |  0.00000 | -0.00000 |  0.00000 |
[XLNX] |     1 |  0.00000 |  0.00000 |  0.00000 |  0.00000 | -0.00000 |  0.00000 |
[XLNX] |     2 |  0.00000 |  0.00000 |  0.00000 |  0.00000 | -0.00000 |  0.00000 |
[XLNX] |     3 |  0.00000 |  0.00000 |  0.00000 |  0.00000 | -0.00000 |  0.00000 |
[XLNX] |     4 |  0.00000 |  0.00000 |  0.00000 |  0.00000 | -0.00000 |  0.00000 |
[XLNX] |     5 |  0.00000 |  0.00000 |  0.00000 |  0.00000 | -0.00000 |  0.00000 |
[XLNX] |     6 |  0.00000 |  0.00000 |  0.00000 |  0.00000 | -0.00000 |  0.00000 |
[XLNX] |     7 |  0.00000 |  0.00000 |  0.00000 |  0.00001 | -0.00000 |  0.00000 |
[XLNX] |     8 |  0.00000 |  0.00000 |  0.00000 |  0.00000 | -0.00000 |  0.00000 |
[XLNX] |     9 |  0.00000 |  0.00000 |  0.00000 |  0.00000 | -0.00000 |  0.00000 |
[XLNX] |    10 |  0.00000 |  0.00000 |  0.00000 |  0.00000 | -0.00000 |  0.00000 |
...
...
...
[XLNX] |   429 |  6.88808 |  0.89559 |  0.03162 |  0.09487 | -0.05605 |  0.24801 |
[XLNX] |   430 | 20.17175 |  0.97100 |  0.00247 |  0.02472 | -0.04771 |  0.76928 |
[XLNX] |   431 | 46.93988 |  0.94174 |  0.00001 |  0.00016 | -0.02719 |  1.41703 |
[XLNX] +-------+----------+----------+----------+----------+----------+----------+
[XLNX] Processed 432 assets in 7293897 us
```
Observe the 7,29 seconds of execution time.

### Step 3 : Hardware Execution Mode

#### Step 3.1: Build the matching CFBlackScholesMerton Kernel (option)
**Attention:** You don´t need to run this step if you already ran the [Black-Scholes-Merton Closed Form Demonstration (Layer 2)](CFBlackScholesMerton_L2_u200.md) in hw mode before.
```
cd /data/Vitis_Libraries/quantitative_finance/L2/tests/CFBlackScholesMerton
make xclbin TARGET=hw DEVICE=xilinx_u200_xdma_201830_2
```

#### Step 2.3: Build host code & run executable
```
cd L3/tests/CFBlackScholesMerton
make run TARGET=hw DEVICE=xilinx_u200_xdma_201830_2
```

## Navigation
Index: [Xilinx´s Quantitative Finances Examples / Walkthrough](quantitative_finance.md)

## Sources
This document is based and extends the following documents:
- https://github.com/Xilinx/Vitis_Libraries/tree/master/quantitative_finance/L3/tests/CFBlackScholesMerton
