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
Kernel repo has been cloned into ./mykernel directory~
