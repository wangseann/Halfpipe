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
# **Specifying Settings in HALFpipe**

Once the pipeline is launched, the user interface will prompt you to enter configuration settings. Below is a detailed breakdown of all options, ensuring you correctly specify inputs for your analysis.

## **1. Data Settings**
### **1.1 Working Directory**
   - **Options**: Enter a file path  
   - **Example**: `/path/to/output_directory`  
   - **Description**: This is the folder where all pipeline output files will be stored.

### **1.2 Is the data in BIDS format?**
   - **Options**: `Yes` / `No`  
   - **Description**: Select `Yes` if your dataset follows the BIDS (Brain Imaging Data Structure) format. If `No`, you will need to specify additional paths manually.

### **1.3 T1-weighted Image Path**
   - **Options**: Enter a file path using `{subject}` placeholder  
   - **Example**: `/path/to/T1/{subject}/T1.nii.gz`  
   - **Description**: The path to anatomical T1-weighted images for each subject. `{subject}` is a placeholder that will be automatically replaced with each subject’s ID.

### **1.4 Resting-State fMRI Data Path**
   - **Options**: Enter a file path using `{subject}` placeholder  
   - **Example**: `/path/to/rsfMRI/{subject}/epi.nii.gz`  
   - **Description**: The path to resting-state fMRI data for each subject. `{subject}` will be replaced dynamically for each subject.

### **1.5 Check Repetition Time (TR)**
   - **Options**: `Yes` / `No`  
   - **Description**: The system will check and display the TR value in seconds. If incorrect, enter the correct TR manually.

### **1.6 Add More BOLD Image Files?**
   - **Options**: `Yes` / `No`  
   - **Description**: Select `Yes` if you have additional functional image files to include.

### **1.7 Slice Timing Correction**
   - **Options**: `Yes` / `No`  
   - **Description**: If `Yes`, specify the slice acquisition order. This is needed if slice timing correction is required in preprocessing.

### **1.8 Field Maps for Distortion Correction**
   - **Options**: `Yes` / `No`  
   - **Description**: If `Yes`, specify the type of field maps used and their paths.

---

## **2. Feature Extraction Settings**
These settings define which resting-state features will be computed. Click on each feature type to view specific configuration instructions.

### **2.1 First-Level Features to Extract**
   - **Options**:  
     - [Seed-Based Connectivity](#seed-based-connectivity)  
     - [Dual Regression](#dual-regression)  
     - [Atlas-Based Connectivity Matrix](#atlas-based-connectivity-matrix)  
     - [ReHo (Regional Homogeneity)](#reho)  
     - [fALFF (Fractional Amplitude of Low-Frequency Fluctuations)](#falff)  
   - **Description**: Choose one or more features to extract during preprocessing.

## **3. Confound Removal and Filtering**
### **3.1 Apply Smoothing**
   - **Options**: `Yes` / `No`  
   - **Description**: If `Yes`, specify the full-width half-maximum (FWHM) smoothing kernel size.

### **3.2 Specify Smoothing FWHM (Full-Width Half-Maximum)**
   - **Options**: Enter a numeric value (e.g., `6.0 mm`)  
   - **Default**: `6.0 mm`  
   - **Description**: Sets the spatial smoothing kernel size in millimeters.

### **3.3 Temporal Filtering**
   - **Options**:  
     - `Skip`  
     - Low-Pass Filter (default: `0 Hz`)  
     - High-Pass Filter (default: `125.0 Hz`)  
   - **Description**: Applies frequency filtering to the time-series data.

### **3.4 Confound Removal**
   - **Options**:  
     - `ICA-AROMA`  
     - `aCompCor`  
     - `Both ICA-AROMA and aCompCor`  
   - **Description**: Specifies the confound regression strategy to remove noise from the signal.

---

## **4. Preprocessing Output Settings**
### **4.1 Output Preprocessed Image**
   - **Options**: `Yes` / `No`  
   - **Description**: If `Yes`, a preprocessed resting-state image will be saved.

### **4.2 Specify Group-Level Model**
   - **Options**: `Yes` / `No`  
   - **Description**: If `Yes`, specify group-level statistical modeling options.

### **4.3 Generate Quality Control Reports**
   - **Options**: `Yes` / `No`  
   - **Description**: If `Yes`, automatically generate reports to check data quality.

---

## **Feature-Specific Configuration Pages**
Each first-level feature has its own dedicated page for detailed configuration.

- [Seed-Based Connectivity](#seed-based-connectivity)
- [Dual Regression](#dual-regression)
- [Atlas-Based Connectivity Matrix](#atlas-based-connectivity-matrix)
- [ReHo (Regional Homogeneity)](#reho)
- [fALFF (Fractional Amplitude of Low-Frequency Fluctuations)](#falff)

---

## **Final Steps**
Once all settings are configured, a **spec.json** file will be generated in the working directory, which contains all specified options. The pipeline will then proceed to process each subject’s data accordingly.

🚀 **You're now ready to run HALFpipe!** Let me know if you need additional refinements.


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


