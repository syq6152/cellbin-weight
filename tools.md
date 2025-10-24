## Introduction to Intelligent Assistant Gadgets

|  Tool Path   | Description                  |
|  ----  |------------------------------|
| stereo3d/tools/saw_hub.py  | Tool for integrating with Stereo-seq Analysis Workflow (SAW) | 
| stereo3d/tools/anno_adata_support.py  | Script for H5AD-formatted single-cell data reconstruction      |
| stereo3d/tools/stereo3d_viewer.ipynb  | 3D visualization viewer of spatial transcriptomics data              |
| stereo3d/stereo3d_with_matrix.py  | After manual image registration, reconnect to the Stereo3D workflow with the -overwriter parameter to generate new results; for matrix-only data, use the paste command to produce registration results with cluster annotations and organ meshes. |
| stereo3d/tools/multi_tissue.py  | Tool for merging and analyzing multi-tissue samples |
| stereo3d/tools/spateo_3d.py  | Tool for interacting with Spateo 3D analysis framework |


## 1. saw_hub
If you have the standard output results from SAW, in addition to manually standardizing the input, you can also use the small tool `saw_hub.py` in Stereo3D to convert the data into the standard input format for Stereo3D. `saw_hub.py` prioritizes obtaining `raw.gef` from the SAW standard output results. If not available, it obtains `gem.gz` and automatically converts `gem.gz` to `gef`.

### Usage
```shell
python saw_hub.py \
-input D:\data\stereo3d\input \
-record D:\code\stereo3d\docs\E-ST20220923002_slice_records_20221110.xlsx \
-stain ssDNA \
-output D:\data\stereo3d\output \
-saw_version 7
```
##### Input Parameters

|  Name   | Description                  | Importance | Dtype  |
|  ----  |------------------------------|------------|--------|
| input  | SAW output dir               | Required   | string |
| record  | Input record sheet path      | Required   | string |
| output  | The output path              | Required   | string |
| saw_version  | The version of SAW (7 or 8)  | Required   | int    |
| stain  | The stain tech (ssDNA or HE) | Required   | string |


##### Output file

|    File Name    |    Description     |
|-----------------|--------------------|
|  gem    |  The [saw](https://github.com/STOmics/SAW) input files(gene matrix)   |
|  mask   | The [saw](https://github.com/STOmics/SAW) output files(`<SN>_<staintype>_tissue_cut.tif`), [details](https://stereotoolss-organization.gitbook.io/saw-user-manual-v8.2/analysis/outputs/count-outputs) |


## 2. anno_adata_support

`anno_adata_support.py` is a tool specifically designed for spatial transcriptomics data analysis, used to integrate already annotated/clustered H5AD format data (Scanpy standard) as a breakpoint into the Stereo3D analysis pipeline. This tool seamlessly integrates external annotation data based on the original Stereo3D analysis results, generating new Stereo3D results while retaining all intermediate results from the original analysis pipeline.

### Usage
```bash
python anno_adata_support.py 
-m
D:\00.user\stereo3D\Drosophila_melanogaster_demo\output\ann_file
-o
D:\00.user\stereo3D\Drosophila_melanogaster_demo\output
-aj
D:\00.user\stereo3D\Drosophila_melanogaster_demo\output\02.register\01.align_mask\align_info.json
-cj
D:\00.user\stereo3D\Drosophila_melanogaster_demo\output\02.register\00.crop_mask\mask_cut_info.json
```

##### Input Parameters 

|  Name   | Description                  | Importance | Dtype  |
|  ----  |------------------------------|------------|--------|
| matrix_path  | File path: Cluster annotations are provided as H5AD files (AnnData objects) with spatial coordinates stored under the `obsm["spatial"]` key | Required   | string |
| output_path  | The output path      | Required   | string |
| align_json_path  | Alignment parameters file: In base pipline, it can be found in `02.register\01.align_mask\align_info.json` if it not provide, coordinates remain unmodified | Optional   | string |
| cut_json_path  | Tissue mask parameters file: In base pipline, it can be found in `02.register\00.crop_mask\mask_cut_info.json` if it not provide, coordinates remain unmodified.  | Optional   | string    |
| cluster_key  |   Cluster label: default = "leiden" , The cluster label name , save in anndata file `.obs` | Optional   | string |

##### Output file

|    File Name    |    Description     |
|-----------------|--------------------|
|  11.tans_adata    | Output H5AD file containing modified x/y coordinates and newly generated z-coordinates    |
|  12.adata_organ         | The new 3D organ files |

For specific operations, please refer to [《Annotation/Clustering Breakpoint Integration and Display》](https://github.com/STOmics/stereo3d/blob/dev/docs/AnnotationClustering%20Breakpoint%20Integration%20and%20Display.md) --- **Use Case: Annotation/Clustering Breakpoint Integration**

## 3. stereo3d_viewer

stereo3d_viewer.ipynb is a Jupyter Notebook file for displaying 3D visualization results. It requires input as h5ad files with unified coordinates, and the files must contain unified cell type annotations or clustering labels.

#### Usage
```shell
# Create a new conda environment named 'jupyter_env' with Python 3.9
conda create -n jupyter_env python=3.9

# Activate the conda environment
conda activate jupyter_env

# Install required packages: Jupyter Notebook, K3D visualization, NumPy, AnnData and seaborn
pip install jupyter notebook k3d numpy anndata seaborn

# Start Jupyter Notebook server
jupyter notebook
```

For specific operations, please refer to [《Annotation/Clustering Breakpoint Integration and Display》](https://github.com/STOmics/stereo3d/blob/dev/docs/AnnotationClustering%20Breakpoint%20Integration%20and%20Display.md) **--- Use Case: Annotation/Clustering Effect Display**

## 4. multi_tissue

Due to small tissue size, multiple sections are mounted on one spatial-temporal chip, or multiple tissues are embedded in one block and then sectioned and mounted, resulting in multiple tissues on one chip. on the data processing side, it needs to be split into individual tissues before being input into the Stereo3D pipeline for reconstruction. For data processing, there are manual splitting solutions and automated splitting solutions.

A tool for splitting multi-tissue samples on spatial transcriptomics chips, which can identify multiple tissue regions on the same chip and segment them into independent units.

a. Manual Splitting Solution primarily uses the tools StereoMap and SAW. For specific operations, please refer to [《Multiple Manual Splitting Solution for One Chip》](https://github.com/STOmics/stereo3d/blob/dev/docs/Multiple%20Manual%20Splitting%20Solution%20for%20One%20Chip.md).

b. Automated Splitting Solution uses the tool `multi_tissue.py` within Stereo3D. For specific operations, please refer to [《Multiple Automatic Splitting Solution for One Chip》](https://github.com/STOmics/stereo3d/blob/dev/docs/Multiple%20Automatic%20Splitting%20Solution%20for%20One%20Chip.md).

### Usage
```shell
python multi_tissue.py \
--mask_path D:\Desktop\stereo3d_1tom\FP200000449TL_C3.tif \
--matrix_path D:\Desktop\stereo3d_1tom\FP200000449TL_C3.gem.gz \
--output D:\Desktop\stereo3d_1tom\output\FP200000449TL_C3
```

##### Input Parameters 
| Name        | Description                 | Importance | Dtype  |
|-------------|-----------------------------|------------|--------|
| matrix      | The path of matrix file     | Required   | string |
| mask        | The path of tissue cut file | Required   | string |
| output      | The output path             | Required   | string |

##### Output file
|    File Name    |    Description     |
|-----------------|--------------------|
|  gem         | matrix files of all tissues |
|  mask     | tissuecut files of all tissues |
|  sn.gef    | Convert gem to gef (only applicable in gem scenarios) |
|  sn.tif         | all-tissue ID representation files |
|  sn_YYYYMMDD.lasso.geojson   |  Label file compatible with stereoMap |


## 5. spateo-3d
Here, we use the spateo framework to build a simple process to input multiple adjacent slices of h5ad files and output the aligned h5ad files and 3D model files. The output format is consistent with the input of spateo-viewer, so the results can be rendered and viewed on it.

## Test dataset
* Test data1, [Drosophila embryos](https://db.cngb.org/stomics/flysta3d/download/)
* Test data2, [Mouse Embryo](https://db.cngb.org/stomics/mosta/download/)

### Installation
* [spateo](https://github.com/aristoteleo/spateo-release)
    ```shell
    # python=3.9
    pip install pymeshfix
    pip install pyacvd
    pip install scanpy
    pip install spateo-release==1.1.1
    ```
* [spateo-viewer](https://github.com/aristoteleo/spateo-viewer)
    ```shell
    git clone https://github.com/aristoteleo/spateo-viewer.git
    cd spateo-viewer
    pip install -r requirements.txt
    ```
   _Note:_ vtk==9.2.2 can solve ```TypeError: Could not find a suitable VTK type for <U54```

### Usage
* spateo
    ```shell
    python spateo_3d.py \
    -input D:\data\stereo3d\spateo\gy\E14-16h_a_count_normal_stereoseq.h5ad \
    -output D:\data\stereo3d\spateo\gy\E14-16h \
    -z_step 1 \
    -slice slice_ID \
    -cluster annotation \
    -cluster_pts 1000
    ```

##### Input Parameters 
  |  Name   | Description                                                                                                                                                                                                                                                                        | Importance | Dtype  |
  |------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------|--------|
  | input  | File or directory path. <br>File path: all slices are in one file; <br>Directory path: all slices are in one directory (the file name must contain numbers to reflect their relationship)                                                                                          | Required   | string |
  | output  | Result save path                                                                                                                                                                                                                                                                   | Required   | int    |
  | z_step  | The distance between slices can be adjusted according to the rendering effect in the spateo-viewer                                                                                                                                                                                 | Optional   | string |
  | cluster  | The field name where the clustering information is located in the h5ad file                                                                                                                                                                                                        | Optional   | int    |
  | slice  | Name the field in the h5ad file that represents the relationship between slices before and after                                                                                                                                                                                   | Optional   | string |
  | cluster_pts  | When there are too many points to describe the 3D tissue expression information, <br>downsampling can be used, and the sampled data is used for process analysis. <br>Here is the number of points sampled for each category,<br>Please use with caution when you have few points. | Optional   | int    |

* spateo-viewer

  * open
    ```shell
    python stv_explorer.py --port 1234
    ```
  * show, [more](https://github.com/aristoteleo/spateo-viewer/blob/main/usage/spateo-viewer.pdf)
