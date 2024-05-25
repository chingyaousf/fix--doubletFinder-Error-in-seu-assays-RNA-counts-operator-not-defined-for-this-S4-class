# **Fix for DoubletFinder Compatibility with Seurat 4.3.0.1**

## **Issue Description**

When using the DoubletFinder package with Seurat version 4.3.0.1, the following error occurs:

`Error in seu@assays$RNA$counts : $ operator not defined for this S4 class.`

This issue arises because the **`doubletFinder`** function attempts to access the **`counts`** slot using the **`$`** operator, which is not defined for the S4 class used in Seurat 4.3.0.1.

## **Environment**

-   **`DoubletFinder`** version: 2.0.4

-   **`Seurat`** version: 4.3.0.1

## **Steps to Reproduce**

1.  Load the necessary packages:

    `suppressMessages(require(DoubletFinder))`

2.  Estimate the number of expected doublets:

    `nExp <- round(ncol(seurat_object) * 0.04) # expect 4% doublets`

3.  Run DoubletFinder:

    `seurat_object <- doubletFinder(seurat_object, pN = 0.25, pK = 0.09, nExp = nExp, PCs = 1:10)`

4.  Encounter the error:

    `Error in seu@assays$RNA$counts : $ operator not defined for this S4 class`

## **The Fix**

To resolve this issue, follow these steps to modify the DoubletFinder source code directly:

### **1. Clone the DoubletFinder Repository**

Open a terminal and clone the DoubletFinder repository from GitHub:

`git clone https://github.com/chris-mcginnis-ucsf/DoubletFinder.git`

### **2. Edit the Relevant Function**

Navigate to the cloned repository and locate the relevant R script files. Specifically, access the **`doubletFinder.R`** script:

`cd DoubletFinder/R`

Open the file **`doubletFinder.R`** (or any other relevant file) in a text editor and replace instances of **`seu@assays$RNA$counts`** with **`seu@assays$RNA@counts`**. For example, change:

`data <- seu@assays$RNA$counts[, real.cells]`

to:

`data <- seu@assays$RNA@counts[, real.cells]`

### **3. Save the Changes**

After making the necessary changes, save the modified R script files.

### **4. Reinstall the Modified Package**

Use the **`remotes`** package to install the modified DoubletFinder package from your local files:

`# Assuming you are in the directory where the modified DoubletFinder code is located remotes::install_local("path/to/DoubletFinder")`

## **Download the Fixed Script**

You can download the fixed doubletFinder.R from my GitHub repository here [fixed_doubletFinder.R](https://github.com/chingyaousf/fix--doubletFinder-Error-in-seu-assays-RNA-counts-operator-not-defined-for-this-S4-class/blob/main/doubletFinder.R "fixed_doubletFinder.R")

I hope this fix is helpful. If you encounter any issues or have questions, please feel free to open an issue on the repository or contact me.
