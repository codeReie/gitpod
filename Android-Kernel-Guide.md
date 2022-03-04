# Guide to Compile Android Kernel~
This one's specifically for Ubuntu based distros and Gitpod workspace.  
Assuming you know a bit of git and bash basics, I'll cover everything else in detail~  

_Note: Guide commands can be followed blindly from any directory. I'll be referring to 64bit ARM SOCs._
### Step 1:
Setup your build environment, dependencies and tools:
```
$ git clone --depth=1 https://github.com/akhilnarang/scripts.git -b master vps
$ cd vps/scripts
$ bash setup/android_build_env.sh
```
_Environment has been setup, let's move onto Kernel part~_
### Step 2:
Clone your kernel repo:
```
git clone --depth=1 <git code url> -b <branch> mykernel
```
For eg (this repo is for Xiaomi's MSM8937 board based devices, "a12/master" named branch has been specified):
```
git clone --depth=1 https://github.com/mi-msm8937/android_kernel_xiaomi_msm8937.git -b a12/master mykernel
```
_Kernel repo has been cloned into ./mykernel directory, let's move onto another interesting part~_
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
_We cloned the toolchain in ./mykernel/toolchain directory, let's get started with actual compilation~_    
  
First we create an out directory (the directory where all of the compiled stuff goes):
```
cd mykernel && mkdir out
```
Now we tell the Makefile the architecture of our device:
```
export ARCH=arm64 && export SUBARCH=arm64
```
_We specified architecture as 64bit ARM._    
  
Now we specify the defconfig that we want to compile for (make sure to choose the one for your device properly):
```
make O=out ARCH=arm64 <defconfig file name>
```
For eg in the MSM8937 kernel repo if I want to compile for "mi8937" device which has it's defconfig named "mi8937_defconfig" then:
```
make O=out ARCH=arm64 mi8937_defconfig
```
_All defconfig files are located in arch/arm64/configs directory (ofcourse the arm64 here means our architecture so look for your designaed paths accordingly, it's `arm` for 32bit btw :P)._    
  
Now the environment needs the path to your compiler to run it during compilation:
```
PATH="${PWD}/toolchain/bin:$PATH"
```
_Note: For any compiler you need to point the path to it's /bin directory. If `clang -v` command shows Proton Clang then the path has been properly set._

To begin the compilation:
```
make -j$(nproc --all) O=out ARCH=arm64 CC=clang LLVM=1 CROSS_COMPILE=aarch64-linux-gnu- CROSS_COMPILE_ARM32=arm-linux-gnueabi-
```
_Note:_ `LLVM=1` _is a shorthand for enabling all LLVM tools instead of GCC binutils. You can instead manually enter commands to use individual tools as per your need if your kernel doesn't support any or the shorthand itself, directly into the_ `make` _command._  

LLVM tool commands (You can use as many as you want, just replace `LLVM=1`):
- `AR=llvm-ar`  
- `NM=llvm-nm`  
- `OBJCOPY=llvm-objcopy`  
- `OBJDUMP=llvm-objdump`  
- `STRIP=llvm-strip`  

_Note: In case compiler throws error regarding defconfig or says /out directory dirty, you can either_ `rm -rf out && mkdir out` _or_ `make clean && make mrproper` _and redo from specifying architecture to makefile step._  
### Step 4:
Congratulations :tada:  
Your kernel has been compiled into ./out/arch/arm64/boot and will probably be in Image.gz-dtb format depending on your tree.  
You can zip it using AnyKernel3 template, edit updater-script, flash, test and release it~  
[AnyKernel3 GitHub Homepage](https://github.com/osm0sis/AnyKernel3)

**Thank You~**
