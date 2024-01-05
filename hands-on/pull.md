## singularity/apptainer pull
### Syntax
Download or build a container from a given URI. 
```
$ singularity/apptainer pull [output file] <URI>
```

### Blast
You can find almost all bioinformatics applications from BioContainer's [Package Index](https://bioconda.github.io/conda-package_index.html)

[Blast](http://blast.ncbi.nlm.nih.gov/Blast.cgi?PAGE_TYPE=BlastDocs) is one of the most popular bioinformatics applications. Here is the guide about how to pull the latest version (2.15.0) from BioContainers and Docker Hub. 

#### Biocontainers
Bioconda is integrated with BioContainers. You can easily find the blast containers from its [bioconda page](https://bioconda.github.io/recipes/blast/README.html#package-blast).

From the instruction, the pull command is listed below:
```
docker pull quay.io/biocontainers/blast:<tag>
(see `blast/tags`_ for valid values for ``<tag>``)
```

You can find the tags by clicking `container` on top of the page as illutrated in the below figure. 

![Biocontainer containers](../images/blast1.png)

There are different tags for each version and multiple containers/tags even exist for the same application version. It is common practice to select the ones that were last modified.

![Biocontainer tags](../images/blast2.png)

To pull the image from BioContainers, we just need to run the below command:
```
## Default
$ singularity pull docker://quay.io/biocontainers/blast:2.15.0--pl5321h6f7f691_1

## To give a customerized output name
$ singularity pull blast_2.15.0.sif docker://quay.io/biocontainers/blast:2.15.0--pl5321h6f7f691_1

$ ls 
    blast_2.15.0.sif blast_2.15.0--pl5321h6f7f691_1.sif 
```

### Pytorch
[PyTorch](https://pytorch.org) is a powerful open-source machine learning framework based on the Python programming language and the Torch library. It's widely used for deep learning, a type of machine learning that builds complex models like artificial neural networks for tasks like image recognition, natural language processing, and more.
If you want to use PyTorch, simply pull the [official container image](https://hub.docker.com/r/pytorch/pytorch) from Docker Hub. 
![Biocontainer tags](../images/pytorch.png)

```
$ singularity pull docker://pytorch/pytorch:2.1.2-cuda11.8-cudnn8-runtime
$ ls
    blast_2.15.0.sif*  blast_2.15.0--pl5321h6f7f691_1.sif*  pytorch_2.1.2-cuda11.8-cudnn8-runtime.sif*
```

## singularity/apptainer shell

### syntax
```
$ singularity/apptainer shell [options] image.sif
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
Singularity> exit
$                          # back to your regular shell prompt
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

