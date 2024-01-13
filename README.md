## What is container?  
- A **container** is an abstraction for a set of technologies that aim to solve the problem of how to get software to run reliably when moved from one computing environment to another.  
- A container **image** is simply a file (or collection of files ) saved on disk that stores everything you need to run a target application or applications.  

<img src="https://miro.medium.com/v2/resize:fit:400/1*OARpkeBkn_Tw3vk8H769OQ.png" alt="Docker" width="200" height="200">

## Docker  
- The concept of containers emerged in 1970s, but they were not well known until the emergence of Docker containers in 2013.  
- Docker is an open source platform for building, deploying, and managing containerized applications.   
- `Some concerns about the security of Docker containers in HPC`: Docker gives superuser privileges, but we do not want users to have full, unrestricted admin/ root access on production systems.  

<img src="https://www.hpcwire.com/wp-content/uploads/2023/01/singularityce.png" alt="Singularity" width="250" height="250">

## Singularity  
Singularity was developed in 2015 as an open-source project by researchers at Lawrence Berkeley National Laboratory led by Gregory Kurtzer.   
Singularity is emerging as the containerization framework of choice in HPC environments.   
1. Enable researchers to package entire scientific workflows, libraries, and even data.
2. Users do not need to ask their system admin (e.g., RCAC) to install software for them.
3. Can use docker images.
4. Secure! 
5. Does not require root privileges.

   
<img src="https://apptainer.org/docs/user/main/_static/logo.png" alt="Apptainer" width="160" height="160">

## Apptainer
In November 2021, the Singularity project joined the Linux Foundation, and renamed to Apptainer. Besides the name change, there are another three major changes:
1. The command is changed to **apptainer** from **singularity**. But the **singularity** command also works, because it is an alias for the command **apptainer**.
2. The variable prefixes are updated to **APPTAINER_** and  **APPTAINERENV_** instead of the previous **SINGULARITY_** and **SINGULARITYENV_** (although both are accepted).
3. The default Apptainer installation is now unprivileged, **allowing unprivileged users to build containers**. 



## Hands-on
- [Load modules](hands-on/load_modules.md)
- [singularity pull](hands-on/pull.md)
- [singularity shell](hands-on/shell.md)
- [singularity run](hands-on/run.md)
- [Build containers](hands-on/build.md)
