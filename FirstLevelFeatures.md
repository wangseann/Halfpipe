# **Specifying First-Level Features in HALFpipe**

This section provides a detailed step-by-step guide for each first-level feature available in HALFpipe. Click on a feature type below to navigate directly to its configuration instructions.

- [Seed-Based Connectivity](#seed-based-connectivity)
- [Dual Regression](#dual-regression)
- [Atlas-Based Connectivity Matrix](#atlas-based-connectivity-matrix)
- [ReHo (Regional Homogeneity)](#reho)
- [fALFF (Fractional Amplitude of Low-Frequency Fluctuations)](#falff)

---

## **1. Seed-Based Connectivity**

### **1.1 Feature Name**
   - **Default**: `seedCorr`
   - **Description**: Computes connectivity between a predefined seed region and the rest of the brain.

### **1.2 Seed Mask Path**
   - **Options**: Enter a file path
   - **Example**: `/path/to/enigma/{seed}_seed_2009.nii.gz`
   - **Description**: Path to the seed masks. `{seed}` will be replaced dynamically with each seed name.

### **1.3 Number of Found Seeds**
   - **Description**: Displays the number of seed images detected. The expected number is `55`.

### **1.4 Minimum Seed Coverage**
   - **Options**: `0.0` to `1.0` (Default: `0.8`)
   - **Description**: Specifies the minimum required seed region coverage in the functional images. Functional images below this threshold are skipped.

### **1.5 Apply Smoothing**
   - **Options**: `Yes` / `No`
   - **Default**: `Yes`, `6.0 mm`
   - **Description**: Applies Gaussian smoothing with a Full-Width Half-Maximum (FWHM) kernel size.

### **1.6 Temporal Filtering**
   - **Options**: `Low-pass`, `High-pass`, or `Skip`
   - **Default**: `High-pass 125.0 Hz`
   - **Description**: Applies frequency filtering using a Gaussian-weighted filter.

### **1.7 Confound Removal**
   - **Options**: `ICA-AROMA`, `aCompCor`, `Both`
   - **Description**: Selects method for removing unwanted signal fluctuations.

### **1.8 Add Another Feature?**
   - **Options**: `Yes` / `No`
   - **Description**: If `Yes`, another seed-based connectivity analysis will be configured with a different confound removal technique.

---

## **2. Dual Regression**

### **2.1 Feature Name**
   - **Default**: `dualReg`
   - **Description**: Computes subject-specific time courses and spatial maps based on ICA template regression.

### **2.2 ICA Template Path**
   - **Options**: Enter a file path
   - **Example**: `/path/to/enigma/{map}_maps_2009.nii.gz`
   - **Description**: Specifies the path to the ICA templates used for regression.

### **2.3 Number of Found ICA Templates**
   - **Description**: Displays the number of ICA templates detected. The expected number is `1`.

### **2.4 Apply Smoothing**
   - **Options**: `Yes` / `No`
   - **Default**: `Yes`, `6.0 mm`
   - **Description**: Applies Gaussian smoothing to the data.

### **2.5 Temporal Filtering**
   - **Options**: `Low-pass`, `High-pass`, or `Skip`
   - **Default**: `High-pass 125.0 Hz`
   - **Description**: Applies frequency filtering to the time series.

### **2.6 Confound Removal**
   - **Options**: `ICA-AROMA`, `aCompCor`, `Both`
   - **Description**: Removes unwanted noise from the signal.

### **2.7 Add Another Feature?**
   - **Options**: `Yes` / `No`
   - **Description**: If `Yes`, another dual regression analysis will be configured.

---

## **3. Atlas-Based Connectivity Matrix**

### **3.1 Feature Name**
   - **Default**: `corrMatrix`
   - **Description**: Computes connectivity matrices using predefined brain atlases.

### **3.2 Atlas Path**
   - **Options**: Enter a file path
   - **Example**: `/path/to/enigma/tpl-MNI152NLin2009cAsym_atlas-{atlas}.nii.gz`
   - **Description**: Path to the atlas file. `{atlas}` will be replaced dynamically.

### **3.3 Number of Found Atlases**
   - **Description**: Displays the number of atlases detected. The expected number is `3`.

### **3.4 Apply Temporal Filtering**
   - **Options**: `Gaussian-weighted`, `Frequency-based`, or `Skip`
   - **Description**: Selects the type of temporal filtering applied.

### **3.5 Confound Removal**
   - **Options**: `ICA-AROMA`, `aCompCor`, `Both`
   - **Description**: Removes noise from time series data.

### **3.6 Add Another Feature?**
   - **Options**: `Yes` / `No`
   - **Description**: If `Yes`, another atlas-based connectivity analysis will be configured.

---

## **4. ReHo (Regional Homogeneity)**

### **4.1 Feature Name**
   - **Default**: `reHo`
   - **Description**: Measures local connectivity by computing the similarity between neighboring voxels.

### **4.2 Apply Smoothing**
   - **Options**: `Yes` / `No`
   - **Default**: `Yes`, `6.0 mm`
   - **Description**: Applies Gaussian smoothing to the data.

### **4.3 Temporal Filtering**
   - **Options**: `Low-pass`, `High-pass`, or `Skip`
   - **Default**: `0.01 Hz to 0.1 Hz`
   - **Description**: Applies frequency-based filtering to enhance signal quality.

### **4.4 Confound Removal**
   - **Options**: `ICA-AROMA`, `aCompCor`, `Both`
   - **Description**: Removes confounds from the signal.

---

## **5. fALFF (Fractional Amplitude of Low-Frequency Fluctuations)**

### **5.1 Feature Name**
   - **Default**: `fALFF`
   - **Description**: Measures the relative amplitude of low-frequency fluctuations in brain activity.

### **5.2 Apply Smoothing**
   - **Options**: `Yes` / `No`
   - **Default**: `Yes`, `6.0 mm`
   - **Description**: Applies Gaussian smoothing to the data.

### **5.3 Temporal Filtering**
   - **Options**: `Low-pass`, `High-pass`, or `Skip`
   - **Default**: `0.01 Hz to 0.1 Hz`
   - **Description**: Applies frequency-based filtering to highlight low-frequency fluctuations.

### **5.4 Confound Removal**
   - **Options**: `ICA-AROMA`, `aCompCor`, `Both`
   - **Description**: Removes confounds from the signal.

---

## **Final Steps**
Once first-level feature settings are configured, the pipeline will extract the requested features. You can specify multiple first-level features and apply different confound removal techniques to each.

🚀 **You're now ready to extract features in HALFpipe!** Let me know if you need additional refinements.

