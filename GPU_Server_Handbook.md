# BIMK GPU Server Handbook V 1.0
__Author:__ Shichen Peng  
__Date:__ 2020-04-14  
__Copyright:__ BIMK Institute of Anhui University  

This file contains all the basic notifications you need to know before you use the GPU server of BIMK.

## Platform 
BIMK has one GPU Server(IP: 210.45.215.199 Port: 22) with 4 NVIDIA Tesla P100 GPU cards which run on the Ubuntu 18.04(bionic) Desktop release. However, the server does not support running any GPU-required programs directly on the low-level system layer. Instead, Singularity as a flexible virtual technique is used to eliminate the problems of software compatibility. Here is a description of why we use Singularity:  
``` 
User A: need to run an old program that requires Pytorch 0.4.0 and python 3.5. 

User B: need to run a new program that requires Pytorch 1.4.0 and python 3.6.  
    ...... 
```
It is hard to manage all these requirements with effect due to the reason that some software can not exist together in the systems layer or it might be an unpractical approach. Fortunately, Singularity tackles this problem by making different software environments to be independent with each other.

## Create Your Account
For security consideration, we do not provide the self-registered service. You need to contact the administrator.

After application and authorization, you can login the GPU server with `ssh`. The initial password is `123456` and you can change it with the following command:
```
passwd
```
CAUTION: We do not provide any root privilige for users. The key reason root in the fact that users with root privilage may tent to have an irreversible damage on GPU server.

## Fast Book of Singularity
Many pre-build Sigularity images are provided at `/Share/singularity/images` and the names of them are organized as `'system-{software1-sofware2-...(option)}-build_date.sif'`.  

### Use Pre-build Images and Run Your Programs
For instance, we have a program file `hijack.py` as follow:
```
#!/usr/bin/env python3
# encoding: utf-8
import torch 
print(torch.cuda.is_avvailable())
```
You can exec this script by using this command:
```
singularity exec ~/PATH/YOUR_IMAGE python3 hijack.py
```

Note that `~/PATH/YOUR_IMAGE` is the path where the image you need and `python3 hijack.py` is the command you want to execute. If you encounter an error like `no gpu device available` but the server resources are sufficient, you can add the command `--nv` after `exec` to solve it.
### Customize Your Image
In general cases, you may need to build your personal images. Singularity provides a simple approach to do it.

1. Step1 Install the Singularity on your local device with root(Linux release)
[How to install the Singularity?](https://sylabs.io/docs/)

2. Step2 Copy the Basic Image `/Share/singularity/images/base-nvidia.sif` to local.
```
scp /Share/singularity/images/base-nvidia.sif  YOUR_ACCOUNT@YOUR_LOCAL_IP:~/YOUR_LOCAL_PATH
```

3. Step3 Create A Writable Image
```
sudo singularity build --sandbox ~/PATH_to_SAVE_IMAGE ~/YOUR_LOCAL_PATH/base-nvidia.sif 
```

4. Step4 Enter the Container of Image
```
sudo singularity shell --writable ~/PATH_to_SAVE_IMAGE
```

5. Step5 Install All You Need as Usual
Such as:
```
sudo apt update && \
sudo apt install python3-pip -y
```

6. Step6 Exit the Container
```
exit
```

7. Step7 Build Your Image
```
sudo singularity build ~/PATH_TO_SAVE/IMAGE_NAME.sif ~/PATH_to_SAVE_IMAGE
```
