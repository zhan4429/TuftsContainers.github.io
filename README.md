Containers have become a transformative technology in HPC, offering significant advantages in portability, reproducibility, performance, isolation, and security. They are rapidly gaining adoption in scientific computing and are poised to play a pivotal role in the future of HPC. 

In this virtual workshop provided by [Tufts Research Technology](https://it.tufts.edu/researchtechnology.tufts.edu), you’ll learn the best practices of pulling and running containers on Tufts HPC, and how to build your own containers with both Apptainer and Docker. 

Topics include:
- What are containers and why should we use them
- How to use Singularity/Apptainer to pull and run containers
- How to build containers with Docker and Apptainer
- Environment modules (ngc and biocontainers) based on containers
- Advanced topics namely multinode-MPI and GPU containers
- Hands-on Demo

## Take home messages
- Always use `singularity/3.8.4` or the lastest singularity module to pull and run containers.
- Use `apptainer/1.2.5-no-suid` only when building containers on the interactive partition.
- Please ensure that you bind `/cluster/tufts` to the container.
- To ensure that you're not wasting your time building your own containers, it's recommended to check if there are any publicly available containers that can serve your purpose.
- If you plan to use GPU containers, ensure compatibility between CUDA version and GPU’s CUDA driver version.
- When running containers that require GPU, make sure to include the `--nv` flag.
- Remember to use `ncdu`, a helpful tool, to regularly check and clean up `$HOME/.apptainer` and `$HOME/.singularity` directories. 


## Hands-on
- [Load modules](hands-on/load_modules.md)
- [singularity pull](hands-on/pull.md)
- [singularity shell](hands-on/shell.md)
- [singularity run](hands-on/run.md)
- [Build containers](hands-on/build.md)
