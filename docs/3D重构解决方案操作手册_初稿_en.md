#The meaning of 3D reconstruction

Three-dimensional (three dimensions, 3D) reconstruction is one of the current research hotspots in the field of life sciences.By constructing a three-dimensional map, the overall distribution of cells and molecules can be restored from the massive transcriptome data of cells, which provides key help in solving many unsolved mysteries in research, such as the analysis of organ functions, the exploration of the mechanism of embryonic development, the study of the pathogenesis of cancer and other complex diseases, and the tracing of the evolution of species.

This technology breaks the limitations of traditional two-dimensional observation and allows researchers to understand the relationship between gene expression, cell interactions and biological phenotypes in a dimension that is closer to the original spatial configuration of biological entities. It opens up new perspectives and paths for basic research and clinical translation in life sciences (such as the discovery of disease diagnostic markers, the design of targeted treatment plans, etc.).

#STOMICS 3D reconstruction solution full process and functional highlights introduction

Traditional spatial transcriptome technology can only provide two-dimensional information of tissue sections and cannot fully present the three-dimensional structure and spatial relationship between cells of biological samples.Even if high-resolution gene expression position detection can be achieved, specialized 3D reconstruction tools are still needed to solve the following challenges:

* Precise spatial registration of consecutive tissue sections

* Integration and standardization of multi-slice data

* 3D visualization of cell types and gene expression patterns

* Reconstruction of cell trajectories and developmental processes across slices


Stereo3D is a three-dimensional reconstruction analysis platform developed by BGI Spatiotemporalomics. It can realize the construction of three-dimensional gene expression maps at the tissue and organ level by integrating multi-slice spatial information, providing a new perspective for developmental biology, disease mechanism research and tissue engineering.

The core advantages of Stereo3D make it a powerful research tool:

* **Extreme versatility: **Flexibly adapts to various scenarios such as image only, matrix only or image and matrix combination to meet different data analysis needs;

* **High efficiency and low cost: **Significantly reduce labor costs and shorten the analysis cycle (the fruit fly Demo is run on a computer with 32G memory, and the results can be obtained within ten minutes).![image](https://alidocs.oss-cn-zhangjiakou.aliyuncs.com/res/jP2lRYKDKaYJ6O8g/img/d8a4d81a-5e5c-4ebc-902b-0b88532cf79e.png)Figure 1 The whole process of 3D reconstruction solution

## Introduction to the wet experiment part (for reference only, attach a link, no detailed introduction, related to the official website)

In the overall workflow of Stereo3D, the wet experiment part is a key link in obtaining original data.This part of the experiment is based on Stereo-seq technology independently developed by BGI, which captures the mRNA information of tissue sections through a spatiotemporal chip.The experimental process includes accurately slicing fresh tissue, attaching it to a spatiotemporal chip, and recording the gene expression data of each site on the chip after reverse transcription and cDNA amplification.BGI supporting kits contain standardized reagents to ensure experimental stability and data reliability.

If you want to know more about products and technical details related to wet experiments, you can visit BGI’s official website:[https://www.bgi.com/](https://www.bgi.com/).

## Introduction to data analysis process

Stereo3D data analysis process takes automation and standardization as its core, and efficiently realizes the transformation from raw data to three-dimensional reconstruction results.At the input level, it supports GEM/GEF format gene expression matrices, tissue segmentation maps, and slice flow tables that record slice locations.

After the process is started, the tissue segmentation map is preprocessed to achieve spatial alignment and build a unified three-dimensional coordinate system; then basic analysis such as matrix conversion and annotation is performed; and finally the registered tissue segmentation map, H5AD data, three-dimensional model (OBJ), etc. are output to help scientific researchers quickly obtain three-dimensional spatial insights and solve the problems of low efficiency and high labor costs of traditional methods.

## Automation solutions

There are currently two automated methods for 3D reconstruction solutions, Stereo3D and Spateo.

Stereo3D's standardized process achieves 3D reconstruction by integrating imaging images and gene expression matrices, and supports 3D reconstruction based on image-only or matrix-based data.If the sample has a complete shape and needs to be combined with tissue morphology analysis, Stereo3D is preferred.

The difference between Spateo and Stereo3D is that it does not rely on imaging images. It is completely based on gene expression matrices and uses statistical generation models to achieve image-free three-dimensional reconstruction.If the sample morphology varies greatly, Spateo is recommended.

### Stereo3D

#### Installation and use

Stereo3D is an open source Python tool that supports deployment in Linux and macOS systems.Here are the quick steps to get started:

1. Installation environment preparation

Please install Anaconda software first ([Anaconda 官方文档](https://docs.anaconda.com/anaconda/install/)) and then do the following:

2. Install Stereo3D```plaintext
# Clone the repository and navigate to the directory
git clone https://github.com/STOmics/stereo3d
cd stereo3d

# Create and activate the Conda environment
conda create --name=stereo3d python=3.8
conda activate stereo3d

# Install dependencies
pip install -r requirements.txt
```3. Run stereo3d\_with\_matrix.py```plaintext
python stereo3d_with_matrix.py \
--matrix_path E:\3D_demo\Drosophila_melanogaster\00.raw_data_matrix\Drosophila_melanogaster_demo\00.gem \
--tissue_mask E:\3D_demo\Drosophila_melanogaster\00.raw_data_matrix\Drosophila_melanogaster_demo\00.mask \
--record_sheet E:\3D_demo\Drosophila_melanogaster\00.raw_data_matrix\Drosophila_melanogaster_demo\E-ST20220923002_slice_records_E14_16.xlsx \
--output E:\3D_demo\Drosophila_melanogaster\00.raw_data_matrix\Drosophila_melanogaster_demo\output
```For more installation details, parameter configuration and case tutorials, please visit[Stereo3D GitHub 仓库](https://github.com/STOmics/stereo3d); If you encounter problems, you are welcome to submit feedback or participate in discussions through GitHub Issues.

#### Introduction to input parameters

If you get the standard output result of SAW, you need to first use the small tool saw\_hub.py in Stereo3D (see 3.2.1 Stereo3D Input Normalization for details) to convert the data into the standard input format of Stereo3D.

Table 1 Introduction to Stereo3D standard input parameters

| **Input** | **Introduction** | **Necessity** | **Data Type** | **Remarks** |
| --- | --- | --- | --- | --- |
| matrix\_path | Gene expression matrix in standard format, supported by raw.gef/gef/gem.gz | Required | string | / |
| tissue\_mask | Tissue segmentation chart, tif format | Required | string | / |
| record\_sheet | Obtained from the experimental end, record the slice position and the corresponding relationship between the previous and next slices | Required | string | / |
| output | result saving path | required | string | / |
| registration | The process performs registration by default. If the input data itself has been registered and no additional registration is required by the algorithm, pass the parameter --registration 0 | Optional | int | / |
| overwriter | If the Stereo3D automatic registration result does not meet the requirements, perform manual registration on the automatically registered file, and then connect the Stereo3D process to output the new result, then pass the parameter --overwriter 0 | Optional | int | For examples, see 2.4.1.1 |
| align | If the input is only a matrix, the matrix reconstruction result can be generated, and only the registered H5AD and organ mesh will be output. Pass the parameter --align paste | Optional | int | For examples, see 3.2.3.2 |

#### Introduction to standard output files

| **Output File** | **Introduction** |
| --- | --- |
| 02.register | Registered tissue mask images after alignment |
| 03.gem | Spatial expression matrix after registration |
| 04.mesh | 3D mesh model reconstructed from clustered point clouds |
| 05.transform | Annotated H5AD file containing spatial coordinates and cell metadata |
| 06.color | H5AD file with unified color mapping for visualization |
| 07.organ | Segmented organ-specific mesh models |

#### View results

After completing the data normalization and running stereo3d\_with\_matrix.py to output the results, you can view the results in the following way.

##### View registration results

Since there is a one-to-one correspondence between images and matrices, matrix transformation reuses the image registration relationship, so you only need to check whether the image registration is correct.

In the output result, the 02.register folder contains 00.crop\_mask (cropped tissue mask) and 01.align\_mask (aligned tissue mask)

| Tools | Version | Introduction | Download link | Function | BGI cloud disk download link |
| --- | --- | --- | --- | --- | --- |
| Fiji | ImageJ 1.54f | Fiji is a free and open source image processing software based on ImageJ that supports fluorescence microscopy images, 3D reconstruction, cell counting and other functions.|[https://imagej.net/software/fiji/downloads](https://imagej.net/software/fiji/downloads)| * View registration results<br> <br>* Manually modify registration results |[https://bgipan.genomics.cn/#/link/OvDFhNolvDYrxCO8l8V0](https://bgipan.genomics.cn/#/link/OvDFhNolvDYrxCO8l8V0)Extract password: RdpK |

Note: Viewed with Fiji software, the files imported to TrakEM2 are displayed in order of file names (according to the default alphanumeric order). Therefore, it is recommended to name the files in the order of the slicing flow table (such as image1.tif, image2.tif) to ensure that the relationship between the front and rear slices is correct.

* Create project: Open Fiji software and create an empty TrakEM2;![image](https://alidocs.oss-cn-zhangjiakou.aliyuncs.com/res/jP2lRYKDKaYJ6O8g/img/c1d1aa22-4925-4356-851a-44ed8a6f18fe.png)* Select directory: Select 01.align\_mask (the directory where the image is located) in the "Select storage folder" window;![image](https://alidocs.oss-cn-zhangjiakou.aliyuncs.com/res/jP2lRYKDKaYJ6O8g/img/455c6505-b78b-45b7-be99-e2149ef09827.png)* Remove other files: Move align\_info.json in the 01.align\_mask folder out of the folder (the folder imported into TrakEM2 can only have pictures, not other files);

* Import folder: Drag the 01.align\_mask folder into "Canvas" (the largest image display area); click "OK" in the "Import directory" window; check "Lock stack" in the "Slice separation" window;![image.png](https://alidocs.oss-cn-zhangjiakou.aliyuncs.com/res/jP2lRYKDKaYJ6O8g/img/3b9a3824-473c-4efa-ab47-086582b7776b.png)
    
    ![image](https://alidocs.oss-cn-zhangjiakou.aliyuncs.com/res/jP2lRYKDKaYJ6O8g/img/0e9a2c8c-86e2-4d68-af92-8c2c3e3d9c8f.png)  
    
    ![image](https://alidocs.oss-cn-zhangjiakou.aliyuncs.com/res/jP2lRYKDKaYJ6O8g/img/5169b0e1-1dc0-4bde-bf98-1737249ea6bd.png)* Adjust the image position: If you need to adjust the display position of the image in "Canvas", first place the mouse on "Preview thumbnail", hold down "Ctrl", and scroll the mouse to adjust the display position of the image in "Canvas"; and you can click the red box of "Preview thumbnail" and move it up, down, left, and right until the image is displayed appropriately;![image.png](https://alidocs.oss-cn-zhangjiakou.aliyuncs.com/res/jP2lRYKDKaYJ6O8g/img/ecb287a8-945a-4780-9373-6fad0962de57.png)* Layer bundling contact: "Patches" window, cancel bundling (that is, the reference image can be positioned later);![image](https://alidocs.oss-cn-zhangjiakou.aliyuncs.com/res/jP2lRYKDKaYJ6O8g/img/68889406-2646-462f-a27c-180d84ddb7df.png)* Switch to the "Layers" window: In the "Layers" window, the number of indicator bars on the left is equal to the number of files in the imported folder.Scroll the mouse, and the visible indicator bar will turn green in turn, and "Canvs" will show different images in turn;![image.png](https://alidocs.oss-cn-zhangjiakou.aliyuncs.com/res/jP2lRYKDKaYJ6O8g/img/08b8781c-a459-4194-87cc-9df6b87f508a.png?x-oss-process=image/crop,x_0,y_0,w_1217,h_817/ignore-error,1)* Image selection: The indicator bar of the reference image is displayed in red, and the indicator bar of the adjusted image is displayed in green; select the indicator bar of the adjusted image, and it will turn green when selected; press the "Shift" key, click other indicator bars in the left column, and the indicator bar of the reference image will be displayed in red;![image.png](https://alidocs.oss-cn-zhangjiakou.aliyuncs.com/res/jP2lRYKDKaYJ6O8g/img/0e3867d4-4945-42ab-9f89-b79f43df30d2.png)* Check the registration results: scroll the mouse to view the registration status of the front and rear frames.![image.png](https://alidocs.oss-cn-zhangjiakou.aliyuncs.com/res/jP2lRYKDKaYJ6O8g/img/d43c124d-259b-40b8-ae72-db16f97403a8.png)##### H5AD result visualization

The output result 06.color is an h5ad file that has unified coordinates and labels, and is visualized using the gadget stereo3d\_viewer.ipynb in Stereo3D.Specific operations can be viewed[《注释/聚类断点接入和展示》](https://alidocs.dingtalk.com/i/nodes/lyQod3RxJK3djBrof2e4bK7dJkb4Mw9r)**\---Usage scenario: Annotation/clustering effect display**

##### mesh/organ visualization

The output result 04.mesh is the entire tissue model, and 07.organ is the organ model, which is visualized using 3D Slicer (the model is stored in obj format).

| Tools | Version | Introduction | Download link | Function |
| --- | --- | --- | --- | --- |
| 3D Slicer | v4.11.20210226 | Supports three-dimensional reconstruction, segmentation and visualization operations on multi-dimensional medical data such as CT and MRI.|[https://download.slicer.org/](https://download.slicer.org/)| * Model visualization<br> <br>* Modify the outer contour of the model |

Note: obj save path cannot be named in Chinese

* Import files and drag the files in the 04.mesh folder into 3D Slicer;![image.png](https://alidocs.oss-cn-zhangjiakou.aliyuncs.com/res/jP2lRYKDKaYJ6O8g/img/657f711c-b4ee-44d7-af77-8279ed9d0f50.png?x-oss-process=image/crop,x_9,y_11,w_736,h_429/ignore-error,1)* View results: In the 3D view, click the center the 3D button in the upper left corner to restore the model position, and scroll the mouse to zoom in/out the model (press the left mouse button to view the model at any angle; press the right mouse button or the mouse wheel to adjust the model size.)![image.png](https://alidocs.oss-cn-zhangjiakou.aliyuncs.com/res/jP2lRYKDKaYJ6O8g/img/a67650b5-cc97-45ba-ba68-534b8a820aa6.png)* Mesh and organ visualization: Drag the files in the 07.organ folder into 3D Slicer and display them together with mesh;![image.png](https://alidocs.oss-cn-zhangjiakou.aliyuncs.com/res/jP2lRYKDKaYJ6O8g/img/afd8a515-930a-4049-95d3-78a46f3fc827.png)

![image.png](https://alidocs.oss-cn-zhangjiakou.aliyuncs.com/res/jP2lRYKDKaYJ6O8g/img/47a72e4e-59c3-4c56-91b5-0e8a785ab271.png)* Color adjustment for model display: In "Models", double-click the color window to pop up the "Slicer" window, select a color, and you can change the display color;![image.png](https://alidocs.oss-cn-zhangjiakou.aliyuncs.com/res/jP2lRYKDKaYJ6O8g/img/9a662521-0611-45ea-99be-4b15cd1d7e5a.png)

![image.png](https://alidocs.oss-cn-zhangjiakou.aliyuncs.com/res/jP2lRYKDKaYJ6O8g/img/bbcb91ab-f3de-4948-93c5-ec158e36b784.png?x-oss-process=image/crop,x_1544,y_51,w_1007,h_689/ignore-error,1)* Partial display of obj: Click the eye icon. If the eyes are closed, they will not be displayed. If the eyes are open, they will be displayed.![image.png](https://alidocs.oss-cn-zhangjiakou.aliyuncs.com/res/jP2lRYKDKaYJ6O8g/img/b7658629-3f34-4df9-baef-9d10a37a811b.png)### Spateo

Spateo focuses on three-dimensional reconstruction of spatial transcriptome data, spatial pattern mining and other computational analysis (such as slice registration, cell trajectory construction); while Spateo-viewer is a supporting visualization tool, specially designed to display Spateo's analysis results, and can interactively present complex three-dimensional spatial structures, gene expression patterns, etc. The two form a complete "analysis-visualization" workflow.

#### Installation and use

Spateo is a tool developed based on Python. The following are the steps to get started quickly:

1. Installation environment preparation

Please install Anaconda software first ([Anaconda 官方文档](https://docs.anaconda.com/anaconda/install/)) and then do the following:

2. Install Spateo and Spateo-view```plaintext
    # Clone the repository and navigate to the directory
    git clone https://github.com/STOmics/stereo3d
    cd stereo3d/stereo3d/tools
    
    # Create and activate the Conda environment
    conda create --name=spateo python=3.9
    conda activate spateo
    
    # Install dependencies
    pip install pymeshfix
    pip install pyacvd
    pip install scanpy
    pip install spateo-release==1.1.1
    ```
    
    ```plaintext
    # Clone the repository and navigate to the directory
    git clone https://github.com/aristoteleo/spateo-viewer.git
    cd spateo-viewer
    
    # Install dependencies
    pip install -r requirements.txt
    
    #Note: vtk==9.2.2 can solve TypeError: Could not find a suitable VTK type for <U54
    ```3. Run spateo\_3d.py and stv\_explorer.py```plaintext
python spateo_3d.py \
-input D:\RD_Data\3D\spateo_data\E14-16h_a_count_normal_stereoseq.h5ad \
-output D:\RD_Data\3D\spateo_data\output \
-z_step 1 \
-slice slice_ID \
-cluster annotation \
-cluster_pts 1000
```

```plaintext
python stv_explorer.py --port 1234
```* For more installation details, parameter configuration and case tutorials, please visit[spateo-release GitHub](https://github.com/aristoteleo/spateo-release)warehouse and[spateo-viewer GitHub](https://github.com/aristoteleo/spateo-viewer)storehouse

*Spateo article:[https://www.cell.com/cell/fulltext/S0092-8674(24)01159-0](https://www.cell.com/cell/fulltext/S0092-8674\(24\)01159-0)

#### Introduction to input parameters

| **Input** | **Introduction** | **Necessity** | **Data Type** |
| --- | --- | --- | --- |
| input | File or directory path.<br>File path: all slices are in one file;<br>Directory path: all slices are in one directory (the file name must contain numbers to reflect the association between slices) | required | string |
| output | result saving path | required | int |
| z\_step | The distance between slices can be adjusted according to the rendering effect in spateo-viewer | Optional | string |
| cluster | The name of the field where the clustering information is located in the h5ad file | Optional | int |
| slice | In the h5ad file, the field name representing the relationship before and after the slice | Optional | string |
| cluster\_pts | When there are too many points used to describe 3D tissue expression information, downsampling can be used and the sampled data can be used for process analysis.Here is the number of points for each type of sampling. Use caution when the number of points is small | Optional | int |

#### Introduction to output files

| **Output File** | **Introduction** |
| --- | --- |
| h5ad | AnnData object processed by count normalization and spatial alignment, containing gene expression matrix and spatial coordinate information. |
| matrices | Raw or preprocessed gene expression matrix files |
| mesh\_models | Segmented organ-specific mesh models |
| pc\_models | Aligned organ-specific point cloud model containing coordinates and attributes of discrete cells/sites in 3D space |
| plot | Visualized results file |
| trans\_h5ad | AnnData after normalization, alignment, and secondary processing, with analysis data |

#### View results

Please check for details[spateo-viewer GitHub](https://github.com/aristoteleo/spateo-viewer)storehouse

## Manual assisted finishing solution

If you are not satisfied with the results of Stereo3D's automated process, you can manually modify the results and return to the process.

#### Manual registration

If the registration result in the Stereo3D automated process results is wrong, you can manually register it and then connect it back to Stereo3D to output the new automated process results.Please see this document for specific operations[《手动配准SOP\_v1》](https://alidocs.dingtalk.com/i/nodes/93NwLYZXWyg6oqO1FvX5EgreJkyEqBQm?corpId=dingdaeb6f85c48618b9f2c783f7214b6d69&iframeQuery=utm_source%3Dportal%26utm_medium%3Dportal_new_tab_open&rnd=0.6391902675229961&sideCollapsed=true&utm_scene=team_space)#### Manually modify mesh

If you are not satisfied with the mesh or organic results of the Stereo3D automated process, you can use 3D slicer to modify them manually.(Since the model results do not affect the results of other files, there is no need to connect back to the Stereo3D process)[《3Dslicer手动修改mesh/organSOP\_v1》](https://alidocs.dingtalk.com/i/nodes/Gl6Pm2Db8D30K1wofqZaoawzJxLq0Ee4?doc_type=wiki_doc)#Application case introduction

## Introduction to test data

| Species Organization | Download Path | File Size |
| --- | --- | --- |
| Drosophila | https://bgipan.genomics.cn/#/link/wSnaxQhJ6i5RTPcYvtgI Extraction password:FJrY | 90 MB |

### Drosophila automated process display

1. Environment configuration```plaintext
        # Clone the repository and navigate to the directory
        git clone https://github.com/STOmics/stereo3d
        cd stereo3d
        
        # Create and activate the Conda environment
        conda create --name=stereo3d python=3.8
        conda activate stereo3d
        
        # Install dependencies
        pip install -r requirements.txt
        ```2. Code execution```plaintext
        python stereo3d_with_matrix.py \
        --matrix_path E:\3D_demo\Drosophila_melanogaster\00.raw_data_matrix\Drosophila_melanogaster_demo\00.gem \
        --tissue_mask E:\3D_demo\Drosophila_melanogaster\00.raw_data_matrix\Drosophila_melanogaster_demo\00.mask \
        --record_sheet E:\3D_demo\Drosophila_melanogaster\00.raw_data_matrix\Drosophila_melanogaster_demo\E-ST20220923002_slice_records_E14_16.xlsx \
        --output E:\3D_demo\Drosophila_melanogaster\00.raw_data_matrix\Drosophila_melanogaster_demo\output
        ```3. Log display![image.png](https://alidocs.oss-cn-zhangjiakou.aliyuncs.com/res/jP2lRYKDKaYJ6O8g/img/5bc75b9e-1d15-4c3a-a6de-a9db3f0ea65d.png)4. Output result display![image](https://alidocs.oss-cn-zhangjiakou.aliyuncs.com/res/jP2lRYKDKaYJ6O8g/img/06219592-7a05-41de-a11f-fcaa2674e456.png)      ![image](https://alidocs.oss-cn-zhangjiakou.aliyuncs.com/res/jP2lRYKDKaYJ6O8g/img/d2d4ab6a-d3af-4e51-8bb0-4902ec4364c3.png)Display of registration results between pairs![image3.gif](https://alidocs.oss-cn-zhangjiakou.aliyuncs.com/res/jP2lRYKDKaYJ6O8g/img/8f80c78c-f3f9-4e42-87c4-41c949739d9b.gif)

Model results display![drosophila_melanogaster.gif](https://alidocs.oss-cn-zhangjiakou.aliyuncs.com/res/jP2lRYKDKaYJ6O8g/img/3dba71ba-c2da-4cca-ba41-41e32bc05a59.gif)Effect display after clustering

## Scene introduction

### Image and matrix scenes

Image and matrix scenes are the standardized process of Stereo3D. For details, please refer to 3.1.1 Drosophila Automation Process Display

### Image scenes only

Stereo3D can be used to generate tissue reconstructions when only images are available.--matrix\_path input can be filled in with a path, no actual matrix is ​​required as input.

Usage```plaintext
    python stereo3d_with_matrix.py \
    --matrix_path E:\3D_demo\Drosophila_melanogaster\00.raw_data_matrix\Drosophila_melanogaster_demo\00.gem \
    --tissue_mask E:\3D_demo\Drosophila_melanogaster\00.raw_data_matrix\Drosophila_melanogaster_demo\00.mask \
    --record_sheet E:\3D_demo\Drosophila_melanogaster\00.raw_data_matrix\Drosophila_melanogaster_demo\E-ST20220923002_slice_records_E14_16.xlsx \
    --output E:\3D_demo\Drosophila_melanogaster\00.raw_data_matrix\Drosophila_melanogaster_demo\output
    ```Output file

| **File Name** | **Description** |
| --- | --- |
| 02.register | Registered tissue mask images after alignment |
| 03.gem | Spatial expression matrix after registration |
| 04.mesh | 3D mesh model reconstructed from clustered point clouds |
| 05.transform | empty |
| 06.color | empty |
| 07.organ | empty |

If one of the structures of an organization wants to be displayed individually, the structures can be grouped into a group and used Stereo3D to generate a structural model.For more details, please submit feedback or participate in the discussion through GitHub Issues.

### Matrix scenes only

When there is only a matrix file and no image, Stereo3D can be used for reconstruction.You can also choose to use Spateo for reconstruction, see 2.3.2 Spateo for details;

#### Stereo3D---Organization Reconstruction

If you want to obtain the mesh of the entire tissue, you can segment the tissue through a matrix, use the generated tissue segmentation mask (mask) as the input of Stereo3D, and finally get its standard output result.

For tissue segmentation using matrix, cellbin2 can be used. See the link for specific operations.[https://github.com/STOmics/cellbin2/blob/main/cellbin2/config/demos/only\_matrix.json](https://github.com/STOmics/cellbin2/blob/main/cellbin2/config/demos/only_matrix.json)After the tissue segmentation results of the matrix are available, the standard input of Stereo3D is available. For the following process, please refer to 3.1.1 Drosophila Automation Process Display

#### Stereo3D---Matrix reconstruction

If you only want to obtain the matrix reconstruction result, you can use the paste parameter in Stereo3D to output the registered H5AD and organ mesh.

Usage```plaintext
    python stereo3d_with_matrix.py \
    --matrix_path D:\RD_Data\3D\Drosophila_melanogaster_demo\00.gem \
    --record_sheet D:\RD_Data\3D\E-ST20220923002_slice_records_E14_16.xlsx \
    --output D:\RD_Data\3D\Drosophila_melanogaster_demo\output \
    --align paste
    ```Input Parameters

| **Name** | **Description** | **Importance** | **Dtype** |
| --- | --- | --- | --- |
| matrix | The path of matrix file | Required | string |
| mask | The path of tissue cut file | Required | string |
| record\_sheet | Record sheet file. We provide you with a sample, click for detail | Required | string |
| output | The output path | Required | string |
| align | Using the paste method to achieve alignment | Required | string |

Output file

| **File Name** | **Description** |
| --- | --- |
| 06.color | H5AD file with unified color mapping for visualization |
| 07.organ | Segmented organ-specific mesh models |

## Introduction to intelligent auxiliary gadgets

Stereo3D is also equipped with a series of practical tools to adapt to different scenarios. The application of these tools in various scenarios will be introduced in detail below.

### Stereo3D input normalization

If you get the standard output result of SAW, in addition to manually normalizing the input, you can also use the small tool saw\_hub.py in Stereo3D to convert the data into the standard input format of Stereo3D.saw\_hub.py first obtains raw.gef from the SAW standard output result, if not, obtains gem.gz, and automatically converts gem.gz into gef.

#### Usage```shell
    python saw_hub.py \
    -input D:\data\stereo3d\input \
    -record D:\code\stereo3d\docs\E-ST20220923002_slice_records_20221110.xlsx \
    -stain ssDNA \
    -output D:\data\stereo3d\output \
    -saw_version 7
    ```#### Input Parameters

| **Name** | **Description** | **Importance** | **Dtype** |
| --- | --- | --- | --- |
| input | SAW output dir | Required | string |
| record | Input record sheet path | Required | string |
| output | The output path | Required | string |
| saw\_version | The version of SAW (7 or 8) | Required | int |
| stain | The stain tech (ssDNA or HE) | Required | string |

#### Output file

| **File Name** | **Description** |
| --- | --- |
| gem | The[saw](https://github.com/STOmics/SAW)input files(gene matrix) |
| mask | The[saw](https://github.com/STOmics/SAW)output files(`<SN>_<staintype>_tissue_cut.tif`),[details](https://stereotoolss-organization.gitbook.io/saw-user-manual-v8.2/analysis/outputs/count-outputs)|

### One-chip multi-scene processing

Because the tissue is too small, if multiple sections are posted on the space-time chip or if multiple tissues are embedded and cut in one embedding block, the result will be multiple tissues on one chip.For this kind of data, xxxxxx is needed on the experimental side (will be added later); on the data side, it needs to be split into individual tissues and then input into the Stereo3D process for reconstruction.For data processing, there are manual splitting schemes and automatic splitting schemes.

1. Manual splitting solution. The main tools used are StereoMap and SAW. Please see the detailed operation.[《一贴多手动拆分方案》](https://alidocs.dingtalk.com/i/nodes/P7QG4Yx2Jp7moAR1H4Ar1DeeV9dEq3XD)2. Automatic splitting plan, using the tools in Stereo3D`multi_tissue.py`, please check the specific operation[《一贴多自动拆分方案》](https://alidocs.dingtalk.com/i/nodes/AR4GpnMqJzM3oe71CKwbxXNlVKe0xjE3)### Annotation/clustering result breakpoint access function`anno_adata_support.py`is a tool specially designed for spatial transcriptome data analysis, used to connect annotated/clustered H5ad format data (Scanpy standard) breakpoints into the Stereo3D analysis process.This tool seamlessly integrates external annotation data to generate new Stereo3D results based on the original Stereo3D analysis results, while retaining all intermediate results of the original analysis process.

Please check the specific operation[《注释/聚类断点接入和展示》](https://alidocs.dingtalk.com/i/nodes/lyQod3RxJK3djBrof2e4bK7dJkb4Mw9r)\---**Usage scenario: Annotation/clustering breakpoint access**

### Add matrix-free image for reconstruction

Adding images without matrix information to improve the 3D model is possible in the Stereo3D process.Completely fill in the sn used for reconstruction in the slice flow table. The relationship between the front and back slices must be correct, and finally run`stereo3d_with_matrix.py`That’s it.```plaintext
python stereo3d_with_matrix.py \
--matrix_path E:\3D_demo\Drosophila_melanogaster\00.raw_data_matrix\Drosophila_melanogaster_demo\01.gem \
--tissue_mask E:\3D_demo\Drosophila_melanogaster\00.raw_data_matrix\Drosophila_melanogaster_demo\00.mask \
--record_sheet E:\3D_demo\Drosophila_melanogaster\00.raw_data_matrix\Drosophila_melanogaster_demo\E-ST20220923002_slice_records_E14_16.xlsx \
--output E:\3D_demo\Drosophila_melanogaster\00.raw_data_matrix\Drosophila_melanogaster_demo\output
```#Q&A

1. If the sample quantity is too small, or the sample quality is poor, should it be discarded?

Answer: If the number of samples is too small, it is best to ensure that the best samples are used for formal experiments to ensure that the key positions are correct (it is best for the customer to confirm the starting and stopping points of the chip patching, as the customer knows his own needs best).Poor sample quality will have little impact on reconstruction, but will have a greater impact on subsequent data analysis, such as diffusion, low capture volume, too few genes, etc. It is recommended not to use poor quality samples.

2. In 3D reconstruction, there is an optional requirement: the number of bin50 sequenced genes in adjacent slices does not exceed 30%. If the bin sizes are different, should corresponding adjustments be made?

Answer: Yes, different bin sizes need to be adjusted accordingly.

3. Which one is more recommended, 3D reconstruction or spateo?

Answer: The input of spateo is an annotated matrix, while Stereo3D uses image (ssDNA) registration, which is a completely different strategy. This has not been specifically evaluated and depends on the user's choice.It feels like you can use image registration first, and then use downstream to fine-tune.

4. What is the score for registration?What score is better, and how to judge whether the registration result is correct?

Answer: Registration scoring is to score the registration results of two adjacent slices.The data score of the mouse embryo project is around 84%, which is quite good.Some registration scores for fruit fly data are 60%, but the registration results are correct.In fact, if the distance between the two pieces is large and the shape difference is large, the registration score may be relatively low.If the score is relatively low, it is best to look at the registration results with the naked eye and make a judgment.(This requires the customer to understand the morphology of the tissue. It was also suggested in the early stage to judge the morphology of the tissue through morphological atlas, MRI or CT; provide a status of the sample before and after embedding during embedding, and it is particularly required that the embedding direction does not deviate significantly from the expected reconstruction direction).

5. If I want to add a few matrix-free ssDNA images to the 3D reconstruction automated process, is it possible?

Answer: It is possible to add ssDNA images to improve the 3D model through automated processes.

6. If the number of samples is too small, or the sample quality is poor, should it be discarded?

Answer: If the number of samples is too small, it is best to ensure that the best samples are used for formal experiments to ensure that the key positions are correct (it is best for the customer to confirm the starting and stopping points of the chip patching, as the customer knows his own needs best).Poor sample quality will have little impact on reconstruction, but will have a greater impact on subsequent data analysis, such as diffusion, low capture volume, too few genes, etc. It is recommended not to use poor quality samples.

7. In terms of high-end journals, tumor size, and the beauty of the model after modeling, how many slices are appropriate?

Answer: There is a relationship between the number of sections and tissue thickness.We should also focus on the tissues and organs we are concerned about, the scientific issues we are concerned about, the hot spots of species and tissues being studied, and the size of our own tissues.For example, if the individual tissue is relatively small (four pieces can be cut), it is enough to ensure that the entire tissue structure can be cut out. There is no need to cut too much.From the perspective of a journal, attention should be paid to its research hot spots.In addition, if you cut more, the proportion of investment in the overall plan of the project will be greater, and the output will naturally be greater.However, if funds are limited, this category must focus on whether the cut area can meet the area of ​​concern, and some integration and mapping can be done in the future. You can cut less and integrate the 3D structure made by previous people.

8. Low resolution means fewer slices; high resolution means more slices.What are the advantages and disadvantages of these two adaptation scenarios?

Answer: For tumor data, the focus is on heterogeneity. If the focus is on the growth structure of the tumor, high resolution is not required.The time period for the previous publication (2023) was relatively early, and the single-cell level could not be reached at that time, so a low-resolution transcriptome structure was chosen at that time.Now, the high resolution can reach the single cell level, and the granularity described is different.If we only do bin100, smaller structures cannot be recognized.Stereo-seq can now reach the single-cell level, and teachers can think from more perspectives and have choices.The more slices the better, it’s more about considering it from the teacher’s perspective and whether it is useful for the teacher to solve the problem.If funds are sufficient, you can consider combining other technologies to make the entire project design more complete and rigorous, not limited to the number of films.If the film meets the teacher's technical requirements, better articles will be published.

9. Between ssDNA and HE staining, if you are doing a 3D project, which staining would you recommend?

Answer: If cellbin is reached, ssDNA will be more stable.

10. If there are a batch of continuous slices, but the thickness of the slices is not the same (for example, some are 10 microns and some are 5 microns), how should the Z-inteval value be set in the Meta information of the slice flow table?


Answer: You need to adjust the Z\_index column in the flow table. This column represents the distance of each slice on the Z axis, not the distance from the previous slice.For example, if the first piece is cut to 5 microns, fill in 5, and if the second piece is cut to 10 microns, fill in 15 microns.(The mesh will be constructed based on the z-interval value)

#Appendix

* If you are interested in Stereo3D and want to know more, please check out related articles:


3D mouse embryo article:

Gastrula Articles:[https://www.sciencedirect.com/science/article/abs/pii/S1934590925001420?via%3Dihub](https://www.sciencedirect.com/science/article/abs/pii/S1934590925001420?via%3Dihub)Spateo article:[https://www.cell.com/cell/fulltext/S0092-8674(24)01159-0](https://www.cell.com/cell/fulltext/S0092-8674\(24\)01159-0)

The 3D reconstruction solution of spatiotemporal omics technology Stereo-seq, video introduction on the official website of BGI Spacetime:[https://www.stomics.tech/resources/Videos/1893.html](https://www.stomics.tech/resources/Videos/1893.html)The spatiotemporal omics technology Stereo-seq assists in the construction of 3D maps, as introduced in the video on the official website of BGI Spacetime:[https://www.stomics.tech/resources/Videos/1892.html](https://www.stomics.tech/resources/Videos/1892.html)* List of third-party software used by Stereo3D


| Software name | Download link | Version number | Function |
| --- | --- | --- | --- |
| ImageStudio |[https://www.stomics.tech/service/stomics-software-download.html](https://www.stomics.tech/service/stomics-software-download.html)| 3.\* | * Perform quality control on image group data to serve SAW registration |
| SAW |[https://www.stomics.tech/service/stomics-software-download.html](https://www.stomics.tech/service/stomics-software-download.html)| V8, V7 and before | * Complete registration of transcript group and image group<br> <br>* Complete tissue segmentation based on image group |
| Stereo3D |[https://github.com/STOmics/stereo3d](https://github.com/STOmics/stereo3d)| v0.0.1 | * Complete the registration between multiple groups of adjacent slices<br> <br>* Unify the coordinate system and generate a 3D model - tissue outer envelope<br> <br>* Output the matrix after registration between multiple slices |
| 3DSlicer |[https://download.slicer.org/](https://download.slicer.org/)| v4.11.20210226 | * Modify the outer contour of the model |
| cloud compare |[https://www.danielgm.net/cc/](https://www.danielgm.net/cc/)| v2.12.4 | * Modify cell annotation internal point cloud |
| blender |[https://www.blender.org/download/](https://www.blender.org/download/)| v4.0 | * Result visualization and rendering |
| Excel | \- | \- | * Make cutter flow chart |