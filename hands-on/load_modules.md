## Start an interactive job

```
$ srun -N1 -n2 -t2:00:00 -p interactive --reservation=rtworkshop --pty bash ## You will start an interactive session on interactive partition with 2 cores and 2-hour walltime.
```

> `--reservation=rtworkshop` is only valid during the workshop. You do not need it in your future commands.

## Singularity
For your convenience, please always use the `singularity` module when you need to pull or run containers on the HPC system. There is no need to use the older singularity modules. As of January 24th, 2024, the latest version available on our cluster is `3.8.4`. In case we deploy newer versions of `singularity` in the future, please make sure to use the latest versions.

It is highly recommended to run `module purge` before loading `singularity`. 
```
$ module purge
$ module load singularity/3.8.4
$ module list
  1) squashfs/4.4                 2) singularity/3.8.4(default)
```

It is worth mentioning two points:
- `singularity/3.8.4` is the default version, meaning you can use either `module load singularity` or `module load singularity/3.8.4`. However, it is recommended to use the latter option. 
- Although you only loaded `singularity/3.8.4`, we modified its modulefile to include `squashfs/4.4` as a dependency.

## Apptainer
Our cluster currently has two groups of `apptainer` modules: one with `-suid` and the other with `-no-suid`. We understand that this might be confusing for users, and we are exploring the best practices to manage both `singularity` and `apptainer` on the same cluster. However, we may ultimately decide to keep only `apptainer` on the cluster.

To avoid confusion, we recommend using `apptainer` only when building containers on the `interactive` partition, and using the `-no-suid` version exclusively. As of January 24th, 2024, version `1.2.5-no-suid` is the latest available version.
```
$ module purge
$ module load apptainer/1.2.5-no-suid
```
[Next: Singularity pull](pull.md)
