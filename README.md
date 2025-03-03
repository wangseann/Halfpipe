# Halfpipe
manual
# ENIGMA HALFpipe User Guide

## **Resting-State Pipeline**
**Written by**: Elena Pozzi, Yara Toenders, Ilya Veer, Lea Waller, Courtney Haswell, Rajendra Morey, and Lianne Schmaal  
**Last Updated**: March 2021  

---

## **Table of Contents**
1. [Introduction](#introduction)
2. [Installation](#installation)
   - [Choosing a Container Platform](#choosing-a-container-platform)
   - [Installing Docker, Singularity, or Apptainer](#installing-docker-singularity-or-apptainer)
3. [Downloading HALFpipe](#downloading-halfpipe)
4. [Running the Pipeline](#running-the-pipeline)
   - [Launching the Pipeline](#launching-the-pipeline)
   - [Specifying Settings](#specifying-settings)
   - [Submitting Jobs on HPC](#submitting-jobs-on-hpc)
5. [Troubleshooting](#troubleshooting)

---

## **1. Introduction**
HALFpipe is a pipeline for processing resting-state fMRI data, using **Docker**, **Singularity**, or **Apptainer** as container platforms. This guide provides step-by-step instructions for installation, execution, and troubleshooting.

For the latest updates, visit the [HALFpipe GitHub Repository](https://github.com/mindandbrain/Halfpipe).

---

## **2. Installation**

### **Choosing a Container Platform**
Select a container platform based on your operating system:

| **Platform**      | **Recommended Container** |
|-------------------|-------------------------|
| Mac OS X         | Docker Desktop          |
| Windows         | Docker Desktop          |
| Linux           | Singularity / Apptainer / Docker  |
| HPC Cluster     | Apptainer (or Singularity) Only  |

### **Installing Docker, Singularity, or Apptainer**
#### **Docker Installation (Mac/Windows/Linux)**
1. Download **Docker Desktop**: [Official Docker Installation Guide](https://docs.docker.com/engine/install/)
2. Install and start Docker on your system.
3. Verify installation:
   ```bash
   docker --version
   ```

#### **Singularity or Apptainer Installation (Linux/HPC)**
1. **For Singularity:**
   ```bash
   sudo apt install singularity-container
   ```
   Verify installation:
   ```bash
   singularity --version
   ```

2. **For Apptainer:**
   ```bash
   sudo apt install apptainer
   ```
   Verify installation:
   ```bash
   apptainer --version
   ```

If using HPC, check if Singularity or Apptainer is pre-installed:
```bash
module load singularity  # or
module load apptainer
```

---

## **3. Downloading HALFpipe**

HALFpipe is approximately **5GB** in size, with an output that can reach **20GB per subject**.

| **Container** | **Download Command** |
|--------------|----------------------|
| **Docker**  | `docker pull halfpipe/halfpipe:1.0.1` |
| **Singularity 3.x** | `singularity pull shub://HALFpipe/HALFpipe:1.0.1` |
| **Singularity 2.x** | `singularity pull docker://halfpipe/halfpipe:1.0.1` |
| **Apptainer** | `apptainer pull docker://halfpipe/halfpipe:1.0.1` |

After downloading, verify installation:
```bash
docker images  # For Docker
singularity image list  # For Singularity
apptainer image list  # For Apptainer
```

---

## **4. Running the Pipeline**

### **Launching the Pipeline**
#### **Local Machine**
**Run one of the following commands based on your container:**
```bash
# Docker
docker run --interactive --tty --volume /:/ext mindandbrain/halfpipe

# Singularity
singularity run --no-home --cleanenv --bind /:/ext HALFpipe_1.0.1.sif

# Apptainer
apptainer run --no-home --cleanenv --bind /:/ext HALFpipe_1.0.1.sif
```

#### **High-Performance Cluster (HPC)**
Before launching HALFpipe, request an interactive job:
```bash
srun --time=0-6 --ntasks=1 --pty bash
module load apptainer  # or module load singularity
```
Run the pipeline on HPC:
```bash
apptainer run --no-home --cleanenv --bind /:/ext HALFpipe_1.0.1.sif --use-cluster
```

---

### **Submitting Jobs on HPC**
Once settings are specified, submit the job:
```bash
sbatch submit.slurm.sh
```
For **SGE clusters**, edit `submit.slurm.sh` and change the job submission format:
```bash
#$ -t 1-82
#$ -pe smp 2
#$ -l h_vmem=24G
#$ -N halfpipe_SITENAME
```

---

## **5. Troubleshooting**
Here are common issues and solutions:

| **Error Message** | **Cause** | **Solution** |
|------------------|----------|-------------|
| `command not found` | Apptainer/Singularity not loaded | Run `module load apptainer` or `module load singularity` |
| `oom-kill event(s) detected` | Memory exceeded | Increase memory allocation (`--mem=64GB`) |
| `singularity: command not found` | Singularity not installed | Install using `sudo apt install singularity-container` |
| `apptainer: command not found` | Apptainer not installed | Install using `sudo apt install apptainer` |

For more troubleshooting, check the [GitHub issues page](https://github.com/mindandbrain/Halfpipe/issues).

---

## **Conclusion**
This guide provides a streamlined and user-friendly approach to running HALFpipe. If you encounter any issues, refer to the **troubleshooting section** or consult the **HALFpipe GitHub repository**.

🚀 **Happy Processing!**


