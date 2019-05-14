# Speckle Pattern Generator
_Robert (Tres) Sims, Fluvio Lobo Fenoglietto, James (Jim) Inziello, Jack Stubbs_

---

## Introduction and Motivation
Speckle tracking is the standard for surface strain/deformation measurement. Measurement strain and strain fields is essential for the proper characterization of a material's mechanical properties.

Voxel-based additive manufacturing allows researchers to 3D print random speckle patterns through the thickness of any material. Additive methods produce parts and assemblies with anisotropic mechanical properties. Furthermore, the highest resolution methods allow for the fabrication of micro-structures that further influence this anisotropic behavior.

To support the characterization efforts of said complex parts and assemblies, here we propose a method for the distribution and fabrication of printed speckles:
* Printed speckels can be distributed randomly along the surface and through the thickness of any input mesh
* The shape and size of the speckles can also be tuned to the user preference

---

## Installation Instructions
The Speckle Pattern Generator (SPG) was developed using [Houdini 17](https://www.sidefx.com/products/houdini/). The repository provides several versions of the **spg_v*.hipnc** file. Each version features distinct capabilities:
* **spg_v1**
* **spg_v2**
* **spg_v3**
* **spg_v4**

---

## Usage
To understand the usage of the SPG, we are supplying an example called **cruciform**. The cruciform geometry is used in biaxial-planar mechanical characterization experiments.

The following steps will guide users through the **spg** using the **cruciform** example:
> **NOTE:** The following guide was developed and has only been tested for **Houdini 17**

> **NOTE:** The following guide expects user familiarity with **Houdini**

0.  **Open spg_v4.hipnc using any Houdini 17 version**
    1.  The **spg** network has been set to **update manually**
        *   We highly recommend that this option is conserved by the user
        *   Certain operations in the network can highjack the computer's RAM (depending on the input model)

1.  **Use _import_geometry_ node to load the cruciform file**
    1.  The default cruciform file path and name are:
        ```
        $HIP/cruciform/w25r12.5.stl
        ```
        > **NOTE:** The **$HIP** prefix is used by **Houdini** to point to the program's main directory
        
        > **NOTE:** Other geometry extensions, such as .OBJ may be supported... but have not been tested...
        
        > **NOTE:** If you DO NOT see the geometry, remember to press `Ctrl + M`
        
![Import Geometry](https://github.com/pd3d/spg/blob/master/media/cruciform_fig_import.PNG)   

2.  **Transform Input Geometry**
    
    This node was created for OCD freaks. Use its `Move Centroid to Origin` button to center the input geometry
    
3.  **Convert Input Geometry from a Surface Mesh to a Voxel-based Volume (Voxelization)**
    *   This node converts the input geometry, tipically in a surface mesh form (.STL, .OBJ), into a voxel-based volume
    *   The process uses [VDB standards](https://www.openvdb.org/about/)
    *   More information about the parameters of this node can be found [here](https://www.sidefx.com/docs/houdini/nodes/sop/vdbfrompolygons.html)
    
    > **NOTE:** The performance of the entire program relies heavily on the **voxel size** parameter specified here. The smaller the size, the higher the resolution, and the longer the wait!
    
![Voxelize Input Geometry](https://github.com/pd3d/spg/blob/master/media/cruciform_fig_voxelize.PNG)

4.  **Erode Volume**
    *   In order to generate speckles embededd within the volume, without intersecting with its surfaces _(a)_, an enclosed, scaled-down         variant _(b)_ of the original volume must be created
    *   The **VDB Reshape SDF** node generates the new embedded volume by _eroding_ the outermost voxels composing the original volume
    *   More information about the parameters of this node can be found [here](https://www.sidefx.com/docs/houdini/nodes/sop/vdbreshapesdf.html)

![Eroding Volume](https://github.com/pd3d/spg/blob/master/media/cruciform_fig_erode.PNG)

---

## Discussion
This section was created to expand the explanation/clarification/discussion of several concepts briefly mentioned above

**_(a)_** The importance of _surface intersections_ depends on the output format/extension of the SPG. **IF the program results in surface meshes** (most common), surface intersections will generate errors, holes, inverted normals. **IF the program results in voxels** (most advanced printers), intersections will be handled inherently.
