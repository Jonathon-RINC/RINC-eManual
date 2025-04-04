---
layout: archive
lang: en
ref: cpp_linux
read_time: true
share: true
author_profile: false
permalink: /docs/en/software/dynamixel/dynamixel_sdk/library_setup/cpp_linux/
sidebar:
  title: DYNAMIXEL SDK
  nav: "dynamixel_sdk"
---

<style>body {counter-reset: h1 4 !important;}</style>
<div style="counter-reset: h2 4"></div>

<!--[dummy Header 1]>
  <h1 id="library-setup"><a href="#library-setup">Library Setup</a></h1>
<![end dummy Header 1]-->

## [CPP Linux](#cpp-linux)

### [Compiler and Builder](#compiler-and-builder)

#### [Compiler](#compiler)

* GNU gcc ver. 5.4.0 20160609 or higher
* To check the version of your gcc compiler:  

  ``` bash
  $ gcc -v 
  ```

* Download the required compiler:  

  ``` bash 
  $ sudo apt-get install gcc-5
  ```

#### [Builder](#builder)

* Build-essential pkg → make
* Download:  

  ``` bash 
  $ sudo apt-get install build-essential
  ```

#### [Dependent Packages](#dependent-packages) 

* Packages needed for cross-compiling 
* Download:  

  ``` bash 
  $ sudo apt-get install gcc-multilib g++-multilib
  ```

#### [Build the Library](#build-the-library)


* Choose which format (32bit or 64bit) do you want to build in. The Makefile is located in the following folder: `[DynamixelSDK folder]/cpp/build/linux32` OR `[DynamixelSDK folder]/cpp/build/linux64` folder for 64-bit platforms OR `[DynamixelSDK folder]/cpp/build/linux_sbc` folder for SBCs.  

  Please note that if you will be building the 32-bit example source, you should build the 32-bit library.

  ![](/assets/images/sw/sdk/dynamixel_sdk/library_setup/cpp/linux/library_file/cpp6.png)


* Go to the Makefile's directory located in `[DynamixelSDK folder]/cpp/build/linux32` OR `[DynamixelSDK folder]/cpp/build/linux64` OR `[DynamixelSDK folder]/cpp/build/linux_sbc` using $ `cd`.

* To build the library file:  

  ``` bash
  $ make
  ```

  ![](/assets/images/sw/sdk/dynamixel_sdk/library_setup/cpp/linux/library_file/cpp1.png)

* If there is an error:  

  ``` bash
  $ make clean && make
  ```

* To delete the library file and object files:  

  ``` bash
  $ make clean
  ```

  ![](/assets/images/sw/sdk/dynamixel_sdk/library_setup/cpp/linux/library_file/cpp2.png)

##### Copy (Install) the Library to the Root Directory

* To make library file and copy it to the root directory (to handle the serial port):  

  ``` bash
  $ sudo make install
  ```

  ![](/assets/images/sw/sdk/dynamixel_sdk/library_setup/cpp/linux/library_file/cpp3.png)

* If there is an error:  

  ``` bash
  $ sudo make uninstall && sudo make install
  ```
 
  OR

  ``` bash
  $ sudo make reinstall
  ```

* To delete the library file from the root directory:  

  ``` bash
  $ sudo make uninstall
  ```

  ![](/assets/images/sw/sdk/dynamixel_sdk/library_setup/cpp/linux/library_file/cpp4.png)

* To recopy the library file to the root directory:  

  ``` bash
  $ sudo make reinstall
  ```

  ![](/assets/images/sw/sdk/dynamixel_sdk/library_setup/cpp/linux/library_file/cpp5.png)

* You will see the built library file in `[DynamixelSDK folder]/c/build/[linuxXX]/libdxl_xYY_cpp.so`

### [Building and Running the Sample Code](#building-and-running-the-sample-code)

The DYNAMIXEL SDK sample code for CPP uses the library files(.so for Linux) built in CPP language.

You should build library files in `[DynamixelSDK folder]/c++/build/[linuxXX]/libdxl_xYY_cpp.so` with its own source code as shown above. 

* Choose which format (32bit or 64bit) do you want to build in. The Makefile file for building source is in `[DynamixelSDK folder]/c++/example/protocol1.0/read_write/linux32` or `[DynamixelSDK folder]/c++/example/protocol1.0/read_write/linux64` folder. If you want to build example source in 32bit, for instance, you should build this library in 32bit as well.

  ![](/assets/images/sw/sdk/dynamixel_sdk/library_setup/cpp/linux/sample_code/excp4.png)

* On the terminal, go to the Makefile located folder `/c++/example/protocol1.0/read_write/linux32`, for example, using `cd`.

* To build executable file, type: 

  ```bash
  $ make
  ```

  ![](/assets/images/sw/sdk/dynamixel_sdk/library_setup/cpp/linux/sample_code/excp1.png)

If it shows some error, try `make clean` and `make` it again.

* To delete executable file, type: 

  ```bash
  $ make clean
  ```

  ![](/assets/images/sw/sdk/dynamixel_sdk/library_setup/cpp/linux/sample_code/excp2.png)

* Make the port available to be used

  ```bash
  $ sudo chmod a+rw /dev/ttyUSB0
  ```

  ![](/assets/images/sw/sdk/dynamixel_sdk/library_setup/cpp/linux/sample_code/excp3.png)

* Run the source code

  ```bash
  $ ./read_write
  ```

  ![](/assets/images/sw/sdk/dynamixel_sdk/library_setup/cpp/linux/sample_code/excp5.png)
