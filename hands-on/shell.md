## singularity shell

### syntax
```
$ singularity shell [options] image.sif
```
### blast
Let's go inside the pulled `blast` container.  

```
$ singularity shell blast_2.15.0.sif  
Singularity> more /etc/os-release 
PRETTY_NAME="Debian GNU/Linux 12 (bookworm)"
NAME="Debian GNU/Linux"
VERSION_ID="12"
VERSION="12 (bookworm)"
VERSION_CODENAME=bookworm
ID=debian
HOME_URL="https://www.debian.org/"
SUPPORT_URL="https://www.debian.org/support"
BUG_REPORT_URL="https://bugs.debian.org/"

Singularity> which blastp
/usr/local/bin/blastp

Singularity> ls /
bin          dev          home         linuxrc      opt          run          srv          usr
boot         environment  lib          media        proc         sbin         sys          var
cluster      etc          lib64        mnt          root         singularity  tmp

Singularity> cd /cluster/

Singularity> ls
home

Singularity> cd /cluster/tufts
bash: cd: /cluster/tufts: No such file or directory
```

You may have noticed that although we can see `/cluster`, `/cluster/tufts` is not inside the container. Only `/cluster/home` is located within the container.

### Exit from the container shell when done inspecting
```
Singularity> exit   # back to your regular shell prompt
```

### Bind /cluster/tufts

To enable the container to access `/cluster/tufts`, we must bind the directory into the container.
```
$ singularity shell -B /cluster/tufts blast_2.15.0.sif 
Singularity> ls /cluster/
home   tufts
Singularity> cd /cluster/tufts/
```

### pytorch
Before using the PyTorch container for our analysis, let's check if `torch` was successfully installed into the Python inside the container. We also need to ensure that we are using the correct Torch version, i.e., 2.1.2.
```
$ singularity shell pytorch_2.1.2-cuda11.8-cudnn8-runtime.sif
$ Singularity> python
Python 3.10.13 (main, Sep 11 2023, 13:44:35) [GCC 11.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import torch
>>> print(torch.__version__)
2.1.2
>>> quit()
Singularity> exit
$ exit # back to your regular shell prompt
```
[Next: Run container](run.md)

[Previous: Load modules](load_modules.md)