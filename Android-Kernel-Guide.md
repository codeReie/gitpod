# Guide to Compile Android Kernel~
This one's specifically for Ubuntu based distros and Gitpod workspace.  
Assuming you know a bit of git and bash basics, I'll cover everything else in detail~
### Step 1:
Setup your build environment, dependencies and tools:
```
git clone --depth=1 https://github.com/akhilnarang/scripts.git -b master
cd scripts
bash setup/android_build_env.sh
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
[Github Homepage](https://github.com/kdrag0n/proton-clang)
