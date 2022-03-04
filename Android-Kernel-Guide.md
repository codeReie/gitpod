# Guide to Compile Android Kernel~
This one's specifically for Ubuntu based distros and Gitpod workspace.  
Assuming you know a bit of git and bash basics, I'll cover everything else in detail~  

Note: Guide commands can be followed blindly from any directory. I'll be referring to 64bit ARM SOCs.
### Step 1:
Setup your build environment, dependencies and tools:
```
$ git clone --depth=1 https://github.com/akhilnarang/scripts.git -b master .
$ cd scripts
$ bash setup/android_build_env.sh
```
Environment has been setup, let's move onto Kernel part~
### Step 2:
Clone your kernel repo:
```
git clone --depth=1 <git code url> -b <branch> mykernel
```
For eg (this repo is for Xiaomi's MSM8937 board based devices, "a12/master" named branch has been specified):
```
git clone --depth=1 https://github.com/mi-msm8937/android_kernel_xiaomi_msm8937.git -b a12/master mykernel
```
Kernel repo has been cloned into ./mykernel directory, let's move onto another interesting part~
### Step 3:
Now we need to select a toolchain to compile our kernel...  
  
What's a toolchain you ask?  
My rough & simplified explanation:
> A toolchain is a set of build tools organized in a chain that compile your project using supplied compilers, linkers, debuggers and libraries recursively and sequentially in an automated fashion.  
  
A few popular toolchains and their flavours:
  - Clang 
    - AOSP Clang
    - Proton Clang
    - Snapdragon Clang
  - GCC
    - Eva GCC
    - GNU's own GCC
  - Uber Toolchain   
  
There might be more but these are all that I know of currently :P  
  
For now I'll explain with Proton Clang which is @kdrag0n's own flavour of Clang:  
[Clang Github Homepage](https://github.com/kdrag0n/proton-clang)  
  
#### Proton Clang:
Let's clone the toolchain:
```
git clone --depth=1 https://github.com/kdrag0n/proton-clang.git -b master mykernel/toolchain
```
We cloned the toolchain in ./mykernel/toolchain directory, let's get started with actual compilation~  
  
First we create an out directory (the directory where all of the compiled stuff goes):
```
cd mykernel && mkdir out
```
Now we tell the Makefile the architecture of our device:
```
export ARCH=arm64 && export SUBARCH=arm64
```
We specified architecture as 64bit ARM.  
  
Now we specify the defconfig that we want to compile for (make sure to choose the one for your device properly):
```
make O=out ARCH=arm64 <defconfig file name>
```
For eg in the MSM8937 kernel repo if I want to compile for "mi8937" device which has it's defconfig named "mi8937_defconfig" then:
```
make O=out ARCH=arm64 mi8937_defconfig
```
All defconfig files are located in arch/arm64/configs directory (ofcourse the arm64 here means our architecture so look for your designaed paths accordingly, it's `arm` for 32bit btw :P).  
  
Now the environment needs the path to your compiler to run it during compilation:
```
PATH="${PWD}/toolchain/bin:$PATH"
```
For any compiler you need to point the path to it's /bin directory.
