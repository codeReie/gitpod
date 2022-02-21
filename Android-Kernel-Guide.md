# Guide to Compile Android Kernel~

This one's specifically for Ubuntu based distros and Gitpod workspace.  
Assuming you know a bit of git and bash basics, I'll cover everything else in detail~

### Step 1:

Setup your build environment, dependencies and tools:

'''  
git clone --depth=1 https://github.com/akhilnarang/scripts.git -b master 
cd scripts
bash setup/android_build_env.sh
'''  

Environment has been setup, let's move onto Kernel part~

### Step 2:

Clone your kernel repo:

'''  
git clone --depth=1 <url> -b <branch> mykernel
'''  

Kernel repo has been cloned into ./mykernel directory~  
