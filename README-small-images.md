# PhysiCell Studio: User Guide

[[full images](README.md)]
[[small images](README-small-images.md)]

PhysiCell Studio is a graphical tool to simplify PhysiCell model editing. It provides a multi-tabbed GUI that allows graphical editing of the model and its associated XML, including the creation/deletion of fundamental objects, e.g., substrates (or signals) in the microenvironment, and cell types. It also lets users run their model and interactively visualize results, allowing for more rapid model refinement.

This document tries to provide brief, but sufficient, guidance on using the Studio - at least its contents (not the challenges involved in developing your particular model). If you experience problems or have questions, please contact us using an appropriate PhysiCell community Slack channel or the Issues section of this GitHub repository. The latter is preferred when reporting a fatal error using the Studio. We also welcome Pull Requests in the Studio's repository (see instructions there) for bug fixes and suggested improvements.

This Guide will be updated as the Studio itself is updated, however there may be a lag. Therefore, if you're running a recent release of the Studio, you may notice some differences in the content described here.

---
## Dependencies

See the README on the Studio GitHub repository for its dependencies.

---
## Running the Studio

There are two primary ways to run the Studio:

1) from a PhysiCell directory: if you have downloaded PhysiCell and are either just learning it, e.g., compiling and running the sample projects, or are actively building your own model, you might run the Studio (installed in another directory) from the command line as follows:
```
python <path-to-Studio-directory>/bin/studio.py -e <name-of-executable-model>
```
Note:
* there are ways to create an alias and/or a symbolic link to shorten this command, depending on your operating system
* you may need to prefix your executable name with `./`, depending on your PATH environment variable
* when you File->Save or Run the model, the configuration file (.xml) will be updated. If you want to retain your original .xml, you should make a copy

2) from the Studio (root) directory:
```
python bin/studio.py
```
Note:
* unless you're running from a "bundled" Studio package which includes a sample executable model, the above command will only let you *edit* a model's .xml and not run an executable. However, you can copy an executable from somewhere else into the Studio root directory and then Run it from there.

A third use case for the Studio is to use it *only* for plotting results (/output) from your model. In that case, you would typically run it from the root PhysiCell directory and in the `Plot` tab, be sure to reference the correct output directory (folder).

---
## Studio Overview


![](./images/tabs_only.png)

[[Config Basics](#config-basics)] [[Microenvironment](#microenvironment)] [[Cell Types](#cell-types)] [[User Params](#user-params)] [[ICs](#ics-initial-conditions)] [[Run](#run)] [[Plot](#plot)] 

---
## Menu: File -> Samples

We load a PhysiCell sample model, the virus-macrophage, to illustrate the contents of the tabs. 

## NOTE: the model (.xml) being loaded from the Studio's `/config` folder has been "flattened". The Studio cannot properly parse a legacy "hierarchical" .xml from PhysiCell where a `cell_definition` may refer to a "parent" in its attributes.

<img src="./images/menu_file_sample_virus.png" width="10%">

---
## Config Basics

<img src="./images/config_virus.png" width="20%">


* === Domain ===
* define the model domain size (we recommend leaving dx=dy=dz=20). A 2D model will have Z range: [-dz/2, dz/2, dz]
* === Times ===
* `Max Time` - of the simulation
* `/Mechanics/Phenotype dt` - 3 time scales in a PhysiCell model (rf. the PhysiCell method paper). Only modify if you're an advanced user.
* === Misc runtime params ===
* `# threads` - # of OpenMP threads (to help speed up calculations)
* `output folder` - where the output files will be written; relative to where you Run the simulation.
* `Save data(intervals)` - there are (primarily) two types of output files saved by PhysiCell: `SVG` (.svg; for cells' positions, sizes, colors) and `Full` (.mat; for substrate concentrations and custom data). Currently, in the Plot tab, when you plot *both* cells and substrates, it assumes those files were written at the same simulation time. Therefore, you should provide the *same* interval value for both if you plan to plot both. However, if you only plan to plot cells' SVG files, then you can set the `Full` interval to a very high value or simply uncheck it to not have any substrate or cells' custom data saved.
* === Initial conditions of cells ===
* `enable` - check if you are providing a text file that contains data for the initial conditions of cells, including their positions, cell types, etc.
* === Cells' global behaviors
*  `virtual walls` - if checked, indicates that cells should bounce away from the domain boundaries when they get too close
*  `disable springs` - if checked, do not perform elastic spring attachments between cells

[ [top](#physicell-studio-user-guide)] [[Config Basics](#config-basics)] [[Microenvironment](#microenvironment)] [[Cell Types](#cell-types)] [[User Params](#user-params)] [[ICs](#ics-initial-conditions)] [[Run](#run)] [[Plot](#plot)]

---
## Microenvironment

<img src="./images/microenv_virus.png" width="20%">

* Define the substrates (or signals) used in the model
* Selecting one in the box on the left will update the parameters on the right.
* The `New` button will create a new substrate with default parameters
* The `Copy` button will create a new substrate with the same parameters as the currently selected substrate
* The `Delete` button will delete the currently selected substrate
* To rename a substrate, double-click it, modify the name, and press the Enter/Return key
* The parameters on the right should be mostly self-explanatory. However, note that the `Dirichlet BC`(Boundary Condition) `Apply to all` button only copies the value provided to each of the boundaries. It does *not* toggle on (enable) those boundaries. You must explicitly enable any boundary that you want to be Dirichlet conditions.
* `calculate gradients` - check if you want substrate gradients to be computed at each "Mechanics dt" timestep. You would need to do so, for example, if certain cell types were chemotaxing (rf. Cell Types | Motility subtab).
* `track in agents` - check if you want cells to keep track of the substrate concentration during secretion

[ [top](#physicell-studio-user-guide)] [[Config Basics](#config-basics)] [[Microenvironment](#microenvironment)] [[Cell Types](#cell-types)] [[User Params](#user-params)] [[ICs](#ics-initial-conditions)] [[Run](#run)] [[Plot](#plot)]

---
## Cell Types

<img src="./images/celltypes_virus.png" width="20%">

* This tab is used to define the phenotype for each cell type and therefore exposes a large number of parameters. Note that it has subtabs, one for each phenotypic cell behavior.
* ...

---
<details>
  <summary><h3>Expand to show Cell Types subtabs</h3></summary>

[[cycle](#cycle)] [[death](#death)] [[volume](#volume)] [[mechanics](#mechanics)] [[motility](#motility)] [[secretion](#secretion)] [[interactions](#interactions)] [[intracellular](#intracellular)] [[custom-data](#custom-data)] 

  ### Cycle
  <img src="./images/cell_cycle_virus.png" width="20%">

   Define the cycle parameters for this cell type.
  
  ### Death
  <img src="./images/cell_death_virus.png" width="20%">

   Define the death parameters for this cell type.

  ### Volume
  <img src="./images/cell_volume_virus.png" width="20%">

   Define the volume parameters for this cell type.

  ### Mechanics
  <img src="./images/cell_mechanics_virus.png" width="20%">

   Define the mechanics parameters for this cell type.

  ### Motility
  <img src="./images/cell_motility_virus_epi.png" width="20%">

  <img src="./images/cell_motility_virus_mac.png" width="20%">

   Define the motility parameters for this cell type.

  ### Secretion
  <img src="./images/cell_secretion_virus_epi.png" width="20%">

   Define the secretion/uptake parameters for this cell type.

  ### Interactions
  <img src="./images/cell_interactions_virus.png" width="20%">

   Define the interactions parameters for this cell type.

  ### Intracellular
  <img src="./images/cell_intracellular_virus.png" width="20%">

   Define the intracellular parameters for this cell type.

  ### Custom Data
  <img src="./images/cell_customdata_virus.png" width="20%">

   Define the custom data parameters for this cell type.


</details>

[ [top](#physicell-studio-user-guide)] [[Config Basics](#config-basics)] [[Microenvironment](#microenvironment)] [[Cell Types](#cell-types)] [[User Params](#user-params)] [[ICs](#ics-initial-conditions)] [[Run](#run)] [[Plot](#plot)]

---
## User Params

<img src="./images/user_params_virus.png" width="20%">


User parameters are general model parameters (as opposed to Cell Types | Custom Data parameters which are specific to cell data). User parameters are accessed in your model's C++ code. Search for `parameters.ints, parameters.doubles`, etc, in various sample projects' `custom.cpp` files. You can click/drag a column separator in this table to change its width. (Unfortunately, that column width information is not retained if you exit the Studio and start it again)

[ [top](#physicell-studio-user-guide)] [[Config Basics](#config-basics)] [[Microenvironment](#microenvironment)] [[Cell Types](#cell-types)] [[User Params](#user-params)] [[ICs](#ics-initial-conditions)] [[Run](#run)] [[Plot](#plot)]

---
# ICs (Initial Conditions)

The ICs tab provides a graphical tool that lets you create fairly simple initial conditions of cells. For now, those ICs are just x,y,z positions and cell type (by name or ID). The currently supported geometric regions in which cells can be placed are boxes (rectangles) and annuli (or disks). The center of either of those regions is specified using x0,y0,z0. R1 and R2 represent the distances/radii. For boxes, R1= (absolute) x-distance from the center to each left/right edge; R2= y-distance from the center to each top/bottom edge; For an annulus: R1= inner radius; R2= outer radius (if R1=0, the shape becomes a disk). Each region can be filled in two different ways: randomly or hex-filled. You only specify the # of cells for the random fill. Note that you can select a cell type from the combobox at the top. The size (radius) of each cell is determined by the Cell Types | Volume (total) parameter.

<img src="./images/r1_r2_box.png" width="15%">   <img src="./images/r1_r2_annulus.png" width="10%">

The typical steps are: select the region type, fill type, # cells (if fill type = random), center of region, R1 and R2. Then click `Plot` to see if they appear where you expect. If not, either click `Undo last` or `Clear all`. Repeat if you have more regions to fill with ICs. When you have the ICs you want, click `Save` to write out the .csv file to the specified folder and filename. The `use cell type names` are the newer (v2 format) way of providing a cells.csv file. If that box is unchecked, the .csv file will be written with cell type IDs instead (v1 format).

In the following image, we demonstrate with a simple example. Here, we have loaded the template model (hence the `default` cell type). With the selected geometric region `annulus/disk`, `random fill`, `# cells` = 100, and the specified center and radii, we click `Plot` to see the result. Note that since R1 > 0, it will indeed be an annulus; if R1=0, we would have a disk.

<img src="./images/ics_template_annulus_100.png" width="20%">

In the following, we create ICs for two cell types, each in a different region.
* `Clear all` to start fresh
* select cell type=`default`; create a hex-filled disk; Plot
* select cell type=`ctype2`; create a hex-filled rectangle; Plot
* if we make a mistake for one of the Plots, use `Undo last`
* provide a unique .csv filename instead of `cell.csv` if you want, and click `Save`

<img src="./images/ics_disk_hex.png" width="20%"><img src="./images/ics_disk_rect.png" width="20%">

The .csv file should contain content that looks similar to the following. Note that since we had `use cell type names` checked, each line will include the name of that cell type. Also, in this case, there will be a single header line at the top that starts with `x` (for the x-coordinate column). If we don't check the `use cell type names`, this is the older style of .csv and it will use cell IDs (integer values) instead of cell type names. And there will not be a header line.
```
x,y,z,type,volume,cycle entry,custom:GFP,custom:sample
-81.2695257531903,-285.4287579015727,0.0,default
-64.44410465728185,-285.4287579015727,0.0,default
-47.618683561373416,-285.4287579015727,0.0,default
-30.793262465464977,-285.4287579015727,0.0,default
...
422.0635527397712,424.27452590563917,0.0,ctype2
380.0,438.8457680040665,0.0,ctype2
396.82542109590844,438.8457680040665,0.0,ctype2
413.65084219181693,438.8457680040665,0.0,ctype2
```
[ [top](#physicell-studio-user-guide)] [[Config Basics](#config-basics)] [[Microenvironment](#microenvironment)] [[Cell Types](#cell-types)] [[User Params](#user-params)] [[ICs](#ics-initial-conditions)] [[Run](#run)] [[Plot](#plot)]

---
# Run

<img src="./images/run_virus.png" width="20%">

[ [top](#physicell-studio-user-guide)] [[Config Basics](#config-basics)] [[Microenvironment](#microenvironment)] [[Cell Types](#cell-types)] [[User Params](#user-params)] [[ICs](#ics-initial-conditions)] [[Run](#run)] [[Plot](#plot)]

---
# Plot

When you start a simulation (using the Run tab) and output files are generated, you can begin visualizing results. In the Plot tab, click `Play` to 
start rendering those results. The `Play` button will switch to `Pause`, so you can halt and restart easily. The two primary objects to visualize are cells and substrates, each with a checkbox toggle. Assuming you selected "SVG" in the `Config Basics` tab ("Save data" section), then .svg files will be written (at the specified time interval of the simulation) and can be plotted when the `.svg` radio button is selected. Your model's C++ code can specify unique cell colors for the SVG (related to, but not necessarily the same as, those in `Legend(.svg)` button). 

Alternatively, you can plot cells' scalar values when the `.mat` radio button is selected. There are many types of scalar variables for cells that are stored in the .mat files. You can see the entire list using the `full list` button, then click the combobox above it. These scalar variables will be a combination of hard-coded ones by PhysiCell and model-specific ones defined in the `Cell Types | Custom Data` subtab. The `partial` button will populate the combobox with a more customary subset of scalar variables. Note that you can select a colorbar for the cells' scalars and can fix lower/upper bounds for the values, if that's desired. Otherwise, the colorbar will be dynamic and use the min/max of the current frame of data.

We show four snapshots of .svg data during the time series of the virus-macrophage simulation.

<img src="./images/plot_virus_t0.png" width="20%"><img src="./images/plot_virus_t1.png" width="20%"><img src="./images/plot_virus_t2.png" width="20%"><img src="./images/plot_virus_t3.png" width="20%">

---
<img src="./images/plot_virus_pressure.png" width="20%">

Plotting the cells' `pressure` scalar (.mat) field.

---

<img src="./images/population_plot_virus.png" width="20%">

The `Population plot` button will plot the cell (types) populations over the entire simulation (or however far it has currently gotten). This will appear in a separate popup window (and sometimes may get hidden behind the main Studio window).

[ [top](#physicell-studio-user-guide)] [[Config Basics](#config-basics)] [[Microenvironment](#microenvironment)] [[Cell Types](#cell-types)] [[User Params](#user-params)] [[ICs](#ics-initial-conditions)] [[Run](#run)] [[Plot](#plot)]

---
# Plot 3D

While not quite as advanced as 2D plotting, it is also possible to run and visualize results from a 3D model. The UI for 3D plots is continuing to evolve. To run a 3D model, you need to use the `-3` or `--3D` argument, e.g.,
```
# if running from the Studio root folder:
python bin/studio.py -e project -c config/simple3D_test1.xml -3
```

<img src="./images/plot3D_cell_types.png" width="20%">

* by default, we plot cells colored by cell type

* use the `View` menu to toggle display of the 3D axes

---

<img src="./images/plot3D_pressure_dynamic.png" width="20%">

* click `partial` to populate the combobox then select `pressure`

* the colorbar range will dynamically update to the min/max of the cells' pressure values for that frame in time

---

<img src="./images/plot3D_pressure_fixed.png" width="20%">
* after checking `fix`, we can manually specify the colorbar range min/max for all frames


### 3D Plotting reminders

For 3D plotting, the following are useful commands to affect the display and mode of interaction:

  * Keypress j / Keypress t: toggle between joystick (position sensitive) and trackball (motion sensitive) styles. In joystick style, motion occurs continuously as long as a mouse button is pressed. In trackball style, motion occurs when the mouse button is pressed and the mouse pointer moves.

  * Keypress c / Keypress a: toggle between camera and actor modes. In camera mode, mouse events affect the camera position and focal point. In actor mode, mouse events affect the actor that is under the mouse pointer.

  * Button 1: rotate the camera around its focal point (if camera mode) or rotate the actor around its origin (if actor mode). The rotation is in the direction defined from the center of the renderer's viewport towards the mouse position. In joystick mode, the magnitude of the rotation is determined by the distance the mouse is from the center of the render window.

  * Button 2: pan the camera (if camera mode) or translate the actor (if actor mode). In joystick mode, the direction of pan or translation is from the center of the viewport towards the mouse position. In trackball mode, the direction of motion is the direction the mouse moves. (Note: with 2-button mice, pan is defined as <shift>-Button 1.)
  
  * Button 3: zoom the camera (if camera mode) or scale the actor (if actor mode). Zoom in/increase scale if the mouse position is in the top half of the viewport; zoom out/decrease scale if the mouse position is in the bottom half. In joystick mode, the amount of zoom is controlled by the distance of the mouse pointer from the horizontal centerline of the window.

  * Keypress 3: toggle the render window into and out of stereo mode. By default, red-blue stereo pairs are created. Some systems support Crystal Eyes LCD stereo glasses; you have to invoke SetStereoTypeToCrystalEyes() on the rendering window.

  * Keypress f: fly to the picked point
  
  * Keypress p: perform a pick operation. The render window interactor has an internal instance of vtkCellPicker that it uses to pick.

  * Keypress r: reset the camera view along the current view direction. Centers the actors and moves the camera so that all actors are visible.

  * Keypress s: modify the representation of all actors so that they are surfaces.

  * Keypress u: invoke the user-defined function. Typically, this keypress will bring up an interactor that you can type commands in.

  * Keypress w: modify the representation of all actors so that they are wireframe.
  

---
# Community Tools

The Studio provides interfaces to other tools used in the broader modeling community. For example, it is possible to export some of your PhysiCell output data into a format for [Simularium](https://simularium.allencell.org/) and then load that file into their [viewer](https://simularium.allencell.org/viewer) to visualilze in your browser. 

<img src="./images/simularium_test1.png" width="20%">
