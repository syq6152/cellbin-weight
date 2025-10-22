# **Manual Registration SOP_v1**

## 1 **Basic Introduction**

● Application Scenario: When the Stereo3D automated registration results do not meet requirements, perform manual registration operations on the automatically registered files, then reconnect to the Stereo3D pipeline to output new results.

● Definition of "Reference Image" and "Registration Image": The reference image is the image used as the coordinate reference and does not undergo transformation itself; the registration image is the image that will be transformed. By default, according to the order in the slice record sheet, the preceding image is the reference image, and the following image is the registration image.

● Note: When viewing with Fiji software and importing into TrakEM2, the images are displayed sorted by filename (according to the default alphanumeric order). Therefore, it is recommended to name the files according to the order in the slice record sheet (e.g., image1.tif, image2.tif) to ensure the correct preceding and subsequent relationship.

## 2 **Tool Description and Preparation**

Since the image and the matrix have a one-to-one correspondence, and the matrix transformation reuses the relationship from image registration, it is sufficient to check whether the image registration is correct.

The output results in the 02.register folder include 00.crop_mask (cropped tissue mask) and 01.align_mask (registered tissue mask).

| Tool | Version      | Introduction                                                 | Download Link                              | Purpose                                                      | BGI Pan Download Link                                        |
| :--- | :----------- | :----------------------------------------------------------- | :----------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| Fiji | ImageJ 1.54f | Fiji is a free, open-source image processing software based on ImageJ, supporting fluorescence microscopy, 3D reconstruction, cell counting, etc. | https://imagej.net/software/fiji/downloads | ● View registration results ● Manually modify registration results | https://bgipan.genomics.cn/#/link/OvDFhNolvDYrxCO8l8V0 Access Code: RdpK |

## 3 **Operation Steps**

### 3.1 **Create Project**

● Create TrakEM2: Open Fiji software and create an empty TrakEM2;

![img](file:///C:\Users\sunyiqiang\AppData\Local\Temp\ksohtml17652\wps24.jpg)

● Select Directory: In the "Select storage folder" window, select the 01.align_mask folder (the directory containing the images);

![img](file:///C:\Users\sunyiqiang\AppData\Local\Temp\ksohtml17652\wps25.jpg)

● Remove Other Files: Move the align_info.json file out of the 01.align_mask folder (**The folder imported into TrakEM2 should only contain images, no other files**);

● Import Folder: Drag the 01.align_mask folder into the "Canvas" (the largest image display area); Click "OK" in the "Import directory" window; In the "Slice separation" window, check the "Lock stack" option;



![img](file:///C:\Users\sunyiqiang\AppData\Local\Temp\ksohtml17652\wps26.jpg) 

![img](file:///C:\Users\sunyiqiang\AppData\Local\Temp\ksohtml17652\wps27.jpg) 

![img](file:///C:\Users\sunyiqiang\AppData\Local\Temp\ksohtml17652\wps28.jpg) 

### 3.2 **Registration Preparation**

● Adjust Image Position: If you need to adjust the display position of the image in the "Canvas", first place the mouse on the "Preview thumbnail", hold down "Ctrl", and scroll the mouse wheel to adjust the image display position in the "Canvas"; You can also click and drag the red box in the "Preview thumbnail" to move it up, down, left, or right until the image display is suitable;

![img](file:///C:\Users\sunyiqiang\AppData\Local\Temp\ksohtml17652\wps29.jpg)

● Unlink Layers: In the "Patches" window, unlink the layers (so that the reference image can subsequently undergo positional transformation);



![img](file:///C:\Users\sunyiqiang\AppData\Local\Temp\ksohtml17652\wps30.jpg) 

● Switch to the "Layers" Window: In the "Layers" window, the number of indicator bars on the left equals the number of files in the imported folder. Scrolling the mouse wheel will cause the indicator bars to turn green alternately, and the "Canvas" will display different images in turn;

![img](file:///C:\Users\sunyiqiang\AppData\Local\Temp\ksohtml17652\wps31.jpg)

● Image Selection: The indicator bar for the reference image is red, and the indicator bar for the image to be adjusted is green; Select the indicator bar of the image to be adjusted (it turns green when selected); Press the "Shift" key and click on another indicator bar in the left panel, then the indicator bar for the reference image will turn red;

![img](file:///C:\Users\sunyiqiang\AppData\Local\Temp\ksohtml17652\wps32.jpg) 

● Check Registration Results: Scroll the mouse wheel to check the registration status between consecutive images.

![img](file:///C:\Users\sunyiqiang\AppData\Local\Temp\ksohtml17652\wps33.jpg)

### 3.3 **Rigid Registration**

Image pairwise rigid registration can be completed through operations such as flipping, translation, and rotation. After registration is completed, output the results according to section 3.5.

First, click on the green image (the registration image). A white rectangular box will appear around it. Then right-click and select Transform → Transform(affine) to start manual rigid registration.

![img](file:///C:\Users\sunyiqiang\AppData\Local\Temp\ksohtml17652\wps34.jpg)

● Flip Operation

If you need to flip the green image (i.e., the image to be adjusted), right-click on the green image, select "Specify transform...", change the value of "Scale in Y" to -1, and click "OK".

Note: Scaling operations are also performed by changing the values of "Scale in X" and "Scale in Y" under "Specify transform...". However, in principle, the unit pixels of the two input masks should correspond to the same physical scale. If the scales of the registration images are found to be different, the input image sizes should be checked.

![img](file:///C:\Users\sunyiqiang\AppData\Local\Temp\ksohtml17652\wps35.jpg)

● Translation and Rotation

Place the mouse on the green image, left-click and drag the green image to achieve translation; Rotate the handle of the clock pointer to achieve rotation around the center of the pointer. You can perform coarse adjustment first, then fine-tune. After registration is completed, right-click and click "Apply Transform".

![img](file:///C:\Users\sunyiqiang\AppData\Local\Temp\ksohtml17652\wps36.jpg)

### 3.4 **Elastic Registration**

If the images have local deformations (such as bending or stretching of biological tissues) and rigid registration cannot meet the requirements, elastic registration can be used to register the deformed areas. In elastic registration, anchor points can "constrain" the transformation of local areas: the area between adjacent anchor points will be smoothly adjusted based on the correspondence of the anchor points, ensuring alignment accuracy under complex deformations. Note that elastic registration should be performed on the basis of completed rigid registration.

If you have just completed the rigid registration of the corresponding images in TrakEM2 and have not closed the workspace, you can skip (1) and (2) below and directly proceed with (3) and subsequent operations in the same interface.

(1) Create Project

The process is the same as in section 3.1 Create Project.

(2) Registration Preparation

The process is the same as in section 3.2 Registration Preparation.

(3) Elastic Registration Operation

The user will complete the registration of the deformed parts by selecting anchor points and moving them.

○ First, click on the green image (the registration image). A white rectangular box will appear around it. Then right-click and select Transform → Transform(non-linear).

○ Anchor the Image: Hold down "Shift" and use the left mouse button to click on the operation points (referred to as "anchor points") on the image for registration. If not necessary, the distance between points should not be too close (for example, in the case of registering mouse embryo cluster images to Blockface images, generally no more than 15 points). It is recommended to anchor the image outline first, then anchor the center, to avoid excessive changes during adjustment (as shown below).

![img](file:///C:\Users\sunyiqiang\AppData\Local\Temp\ksohtml17652\wps37.jpg)

○ Select Anchor Points for Local Adjustment: Click the left mouse button and drag the anchor points so that the same tissue areas of the green image and the red image overlap on the canvas, achieving elastic registration, as shown below.

![img](file:///C:\Users\sunyiqiang\AppData\Local\Temp\ksohtml17652\wps38.jpg) 

○ Other Operations: If you want to restart the registration operation during adjustment, right-click and select "Cancel Transform"; If you want to delete a single anchor point, press "Ctrl+Shift" and click on the anchor point.

○ After registration is completed, right-click and click "Apply Transform";

### 3.5 **Output Registration Results**

After manual registration is completed, save the registration relationships and the registered images. (Number of adjusted images = Number of registration relationships)

● Save Registration Relationships

Right-click, select "Project", click "Save as...", and name the file sn.xml (sn = filename of the adjusted image)

![img](file:///C:\Users\sunyiqiang\AppData\Local\Temp\ksohtml17652\wps39.jpg)

![img](file:///C:\Users\sunyiqiang\AppData\Local\Temp\ksohtml17652\wps40.jpg)

● Save Registered Images

Right-click, select "Export", click "Make flat image…";

![img](file:///C:\Users\sunyiqiang\AppData\Local\Temp\ksohtml17652\wps41.jpg)

In the pop-up "Choose" window, change the "Type" option to "RGB Color" or "8-bit grayscale" (depending on the nature of the imported image); Change the "Export" option to "Save to file" (as shown below). Click "OK", and save to the same location as above;

![img](file:///C:\Users\sunyiqiang\AppData\Local\Temp\ksohtml17652\wps42.jpg)

After the file is automatically saved, find the file in the save path and rename it to "sn_manual".

![img](file:///C:\Users\sunyiqiang\AppData\Local\Temp\ksohtml17652\wps43.jpg)

## 4 **Reconnect to Stereo3D Pipeline**

After manually registering the images, you need to reconnect to the Stereo3D pipeline to generate new results.

● The file structure after registration is as follows: The trakem2 folder generated during manual registration should be deleted; align_info.json is in the 01.align_mask folder;

![img](file:///C:\Users\sunyiqiang\AppData\Local\Temp\ksohtml17652\wps44.jpg)

● In the result file's 02.register folder, create a new subfolder named 02.manual, and place the registration relationship file sn.xml inside it;

![img](file:///C:\Users\sunyiqiang\AppData\Local\Temp\ksohtml17652\wps45.jpg)

● Place the exported registered image sn_manual.tif into the 01.align_mask folder, and remove the original sn.tif;

![img](file:///C:\Users\sunyiqiang\AppData\Local\Temp\ksohtml17652\wps46.jpg)

● Delete the result folders from 03.gem to 07.organ

![img](file:///C:\Users\sunyiqiang\AppData\Local\Temp\ksohtml17652\wps47.jpg)

● Use the `--overwriter` parameter. The command is as shown below, which generates new 3D results based on the manual registration results.

text

```
python stereo3d_with_matrix.py \
--matrix_path D:\RD_Data\3D\Drosophila_melanogaster_demo\00.gem \
--tissue_mask D:\RD_Data\3D\Drosophila_melanogaster_demo\00.mask \
--record_sheet D:\RD_Data\3D\E-ST20220923002_slice_records_E14_16.xlsx \
--output D:\RD_Data\3D\demo_output_20250416 \
--overwriter 0
```