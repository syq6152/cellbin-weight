# **Annotation/Clustering Breakpoint Integration and Display**

### **Use Case: Annotation/Clustering Breakpoint Integration**

Use Case: Users have annotated/clustered h5ad data (h5ad has unified cell types or cluster labels, scanpy format) that needs to be integrated into the stereo3d analysis pipeline.

Requirement: The input h5ad files are already sorted in the correct order.

Tool: anno_adata_support.py; By default, it does not change the input h5ad colors.

##### **Input Parameters:**

##### Input Parameters:

`--matrix_path` (str, required)
File path: Cluster annotations are provided as H5AD files (AnnData objects) with spatial coordinates stored under the `obsm["spatial"]` key

`--output_path` (str, required)
Output Path

`--align_json_path` (str)
Alignment parameters file: In base pipline, it can be found in `02.register\01.align_mask\align_info.json` if it not provide, coordinates remain unmodified.

`--cut_json_path` (str)
Tissue mask parameters file: In base pipline, it can be found in `02.register\00.crop_mask\mask_cut_info.json` if it not provide, coordinates remain unmodified.

```
--cluster_key` (str)
Cluster label: default = "leiden"
The cluster label name , save in anndata file `.obs
```

##### **Output file:**

##### Output file:

`11.tans_adata` :
Output H5AD file containing modified x/y coordinates and newly generated z-coordinates.

`12.adata_organ` :
The new 3D organ files.

For detailed information, please refer to the usage instructions:
https://github.com/STOmics/stereo3d/blob/dev/stereo3d/tools/README.md



##### Operation Steps:

1. Open the terminal, pull the code, set up the environment, and install packages

```
# Clone the development branch of the stereo3d repository
git clone https://github.com/STOmics/stereo3d.git

# Create a new conda environment named 'stereo3d' with Python 3.8
conda create --name=stereo3d python=3.8

# Activate the newly created conda environment
conda activate stereo3d

# Navigate into the cloned repository directory
cd stereo3d

# Install all required dependencies from the requirements file
pip install -r requirements.txt
```

2. Locate the tool tools/anno_adata_support.py and run it

If the input h5ad files do not have unified coordinates, use this example:

```
cd stereo3d/tools
python anno_adata_support.py -m D:\RD_Data\3D\test\ann_file -o D:\RD_Data\3D\output_aj -aj D:\RD_Data\3D\demo_output_20250416\02.register\01.align_mask\align_info.json -cj D:\RD_Data\3D\demo_output_20250416\02.register\00.crop_mask\mask_cut_info.json
```

If the input h5ad files already have unified coordinates, use this example:

```
cd stereo3d/tools
python anno_adata_support.py -m D:\RD_Data\3D\demo_output_20250416\06.color -o D:\RD_Data\3D\output_aj
```

3. Running interface and output results

Interface display after running completion

![img](file:///C:\Users\sunyiqiang\AppData\Local\Temp\ksohtml17652\wps56.jpg)

Output results interface display

![img](file:///C:\Users\sunyiqiang\AppData\Local\Temp\ksohtml17652\wps57.jpg)

### **Use Case: Annotation/Clustering Effect Display**

Prerequisite: The h5ad files have unified coordinates and unified cell types or cluster labels

Operation Steps:

1. Open the terminal and navigate to the path where stereo3d_viewer.ipynb is stored

For example: If K3D_show.ipynb is stored in D:\code\3d, open the terminal, switch to the D drive, then cd into D:\code\3d

```
D:
cd  D:\code\3d
```

2. Install the Python 3.9 environment (supports jupyter), then install packages, and run jupyter notebook
 ```
   # Create a new conda environment named 'jupyter_env' with Python 3.9
   conda create -n jupyter_env python=3.9
   
   # Activate the conda environment
   conda activate jupyter_env
   
   # Install required packages: Jupyter Notebook, K3D visualization, NumPy, AnnData and seaborn
   pip install jupyter notebook k3d numpy anndata seaborn
   
   # Start Jupyter Notebook server
   jupyter notebook
 ```

3. A browser window will pop up, double-click stereo3d_viewer.ipynb

![img](file:///C:\Users\sunyiqiang\AppData\Local\Temp\ksohtml17652\wps58.jpg)

4. Fill in the path to the h5ad file

![img](file:///C:\Users\sunyiqiang\AppData\Local\Temp\ksohtml17652\wps59.jpg)

5. Click "**Run this Cell and Advance**" to run the code. Click once to run one cell. The red boxes in the image below from left to right are: **Run this Cell and Advance**, **Interrupt the Kernel**, **Restart the Kernel**, **Restart the Kernel and Run All Cells**![img](file:///C:\Users\sunyiqiang\AppData\Local\Temp\ksohtml17652\wps60.jpg)
6. Display effect of the fruit fly demo data

![img](file:///C:\Users\sunyiqiang\AppData\Local\Temp\ksohtml17652\wps61.jpg)<img src="https://alidocs.dingtalk.com/core/api/resources/img/5eecdaf48460cde580806160b83883231fce8580e72055ef75b8339e1c4c24831f5a582afd55f51d8a3a5b4275315d43a156a98577f418d5168508ca19ce09e3a9f928ba2ee9c37643da02ac2150bbb0f99a68d6b7a44b3e2c7bc65b6503c0b1?tmpCode=160ba6ad-95fa-4a62-a62b-de04aba881d2" alt="img" style="zoom:50%;" />



7. Press Ctrl+C in the terminal to exit jupyter notebook
8. If you need to adjust the point size, modify "point_size: float = 0.01" (the larger the value, the larger the points; location as shown below)

![img](file:///C:\Users\sunyiqiang\AppData\Local\Temp\ksohtml17652\wps63.jpg)

9. If you need to adjust the slice interval, modify "z_intervel = 0.02" (the larger the value, the larger the slice interval; location as shown below)

![img](file:///C:\Users\sunyiqiang\AppData\Local\Temp\ksohtml17652\wps64.jpg)