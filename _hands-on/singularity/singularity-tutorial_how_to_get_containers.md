---
topic: singularity
title: Advanced tutorial - How to get containers
---

# How to get containers

💬 Building containers from scratch requires root privileges, so it can not be done on Puhti.

- Instead, you will have to import a ready image file (or use [Tykky](https://docs.csc.fi/computing/containers/tykky/) if containerizing a Conda/pip environment). There are various options to do this.

## 1. Run or pull an existing Singularity container from a repository

1. It is possible to run containers directly from a repository:

```bash
apptainer run shub://vsoch/hello-world:latest
```

- This can, however, lead to a batch job failing if there are network problems.

{:start="2"}
2. Usually it is better to pull the container first and then use the image file:

```bash
apptainer pull shub://vsoch/hello-world:latest
apptainer run hello-world_latest.sif
```

## 2. Convert an existing Docker container to an Apptainer container

💬 Docker images are downloaded as layers. These layers are stored in a cache directory.

- Default location of the cache is `$HOME/.singularity/cache`.
- Since the home directory has limited capacity and some images can be large, it's best to set `$SINGULARITY_CACHE` to point to some other location with more space.

### Option A

1. If you're running on a node with no fast local storage, you can use e.g. `/scratch`:

```bash
export APPTAINER_TMPDIR=/scratch/<project>/$USER      # replace <project> with your CSC project, e.g. project_2001234
export APPTAINER_CACHEDIR=/scratch/<project>/$USER    # replace <project> with your CSC project, e.g. project_2001234
```

### Option B

1. If you're running interactively or as a batch job on an I/O node, you can use the fast local storage:

```bash
export APPTAINER_TMPDIR=$LOCAL_SCRATCH
export APPTAINER_CACHEDIR=$LOCAL_SCRATCH
```

{:start="2"}
2. Avoid some unnecessary warnings by unsetting a certain environment variable:

```bash
unset XDG_RUNTIME_DIR
```

{:start="3"}
3. You can now run `singularity build`:

```bash
apptainer build alpine.sif docker://library/alpine:latest
```

💡 You can find more detailed instructions on converting Docker containers in [Docs CSC](https://docs.csc.fi/computing/containers/creating/#converting-a-docker-container).

## 3. Build the container on another system and transfer the image file to Puhti

‼️ To do this you will need access to a system where you have root privileges and that has Apptainer installed.

1. You can check the current Apptainer version on Puhti with:

```bash
apptainer --version
```

{:start="2"}
2. After creating an image file, you can transfer it to Puhti.

## More information

💬 This tutorial is meant as a brief introduction to get you started.

☝🏻 When searching online for instructions, pay attention that the instructions are for the same version of Apptainer as you are using. There has been some command syntax changes etc. between versions so older instructions may not work as copy/paste. Also note that Apptainer is formerly known as Singularity.

💡 For more detailed instructions, see the [Apptainer documentation](https://apptainer.org/docs/user/latest/).
