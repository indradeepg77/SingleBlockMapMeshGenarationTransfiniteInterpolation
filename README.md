# Single Block 3D Map Mesh Generation by Transfinite Interpolation
Generation of singular block structured mesh and export data in .plot3d format by transfinite interpolation
* Write a program that generates a structured mesh in 3D, given the boundary surface (triangulated) of the domain.

* Inputs:
1. A single surface (in tria format) is provided as the input to the program. (See below for formats)
2. No. of nodes of the mesh in 3 directions: Nx, Ny and Nz
3. Interpolation/scheme type: 0 for Laplacian smoothing, 1 for Transfinite Interpolation.


Steps:
2. Input triangulated surface would have 12 feature edges/curves. Evaluate these feature curves from the input surface.
3. Split the surface into 6 patches using these curves and arrange them in an order i.e. I_min, I_max, J_min, J_max, K_min and K_max patches.
4. Parameterize the 12 edges using arc length.
5. Interpolate (using Transfinite Interpolation) these edges to create the 6 surface meshes and project them onto the patches.
6. Interpolate these surface meshes to generate a structured mesh in the interior, using TFI.

* Output:
     * A structured mesh with Nx X Ny X Nz nodes, in a simple format (like plot3d). (See below for formats)
     * A stack of Nz 2d triangular meshes with buondray Nx X Ny boundary subdivisions. The triangular mesh will be generated by sudividing 2d quads into 2d trias.

* File Formats:
     * Tria: (Input)
         * Number of points on the surface <N>
	 * Coordinates of these points
	 * No. of triangles <T>
	 * Indices of nodes of each of the triangle

 	 <N>
	 <coords of p0>
	 <coords of p1>
	 ...
 	 <coords of p_N-1>
	 <T>
	 <indices of T1> followed by 0 (thickness)
	 <indices of T2> followed by 0 (thickness)
	 ....

	 Eg:
	 8
	  1.0000000000000000  1.0000000000000000  1.0000000000000000
	  1.0000000000000000  1.0000000000000000 -1.0000000000000000
	 -1.0000000000000000  1.0000000000000000  1.0000000000000000
	 -1.0000000000000000  1.0000000000000000 -1.0000000000000000
	  1.0000000000000000 -1.0000000000000000  1.0000000000000000
	  1.0000000000000000 -1.0000000000000000 -1.0000000000000000
	 -1.0000000000000000 -1.0000000000000000  1.0000000000000000
	 -1.0000000000000000 -1.0000000000000000 -1.0000000000000000
	 12
	 1 2 4  0
	 1 4 3  0
	 5 6 2  0
	 5 2 1  0
	 1 3 7  0
	 1 7 5  0
	 7 8 6  0
	 7 6 5  0
	 3 4 8  0
	 3 8 7  0
	 6 8 4  0
	 6 4 2  0

     * plot3d: (output)
         * No. of blocks of the grid (In this case, it is 1)
         * Number of nodes in i, j and k directions (Nx, Ny and Nz)
	 * Coordinates of the nodes of the grid using nested for loops

	 1
	 <Nx> <Ny> <Nz>
	 for i in [0, Nx]:
	     for j in [0, Ny]:
	         for k in [0, Nz]:
		         Xn Yn Zn

	 Eg:
	 1
         5       3       4
	 1.00000000000e+00  1.00000000000e+00  1.00000000000e+00  1.00000000000e+00 
	 1.00000000000e+00 -1.35166912522e-03 -1.19207089355e-03 -1.14787131759e-03 
	 -1.19225640839e-03 -1.35125631744e-03 -1.00000000000e+00 -1.00000000000e+00 
	 -1.00000000000e+00 -1.00000000000e+00 -1.00000000000e+00  1.00000000000e+00 
	 1.00000000000e+00  1.00000000000e+00  1.00000000000e+00  1.00000000000e+00 
	 -1.16958002406e-03 -1.13757562097e-03 -1.12172546366e-03 -1.13748205279e-03 
	 -1.16942505064e-03 -1.00000000000e+00 -1.00000000000e+00 -1.00000000000e+00 
	 -1.00000000000e+00 -1.00000000000e+00  1.00000000000e+00  1.00000000000e+00 
	 1.00000000000e+00  1.00000000000e+00  1.00000000000e+00 -1.16971572639e-03 
	 -1.13754683115e-03 -1.12165487437e-03 -1.13745249885e-03 -1.16956041044e-03 
	 -1.00000000000e+00 -1.00000000000e+00 -1.00000000000e+00 -1.00000000000e+00 
	 -1.00000000000e+00  1.00000000000e+00  1.00000000000e+00  1.00000000000e+00 
	 1.00000000000e+00  1.00000000000e+00 -1.35124502766e-03 -1.19185902823e-03 
	 -1.14774533995e-03 -1.19204391693e-03 -1.35082629092e-03 -1.00000000000e+00 
	 -1.00000000000e+00 -1.00000000000e+00 -1.00000000000e+00 -1.00000000000e+00 
	 1.00000000000e+00  4.99046511179e-01 -1.35407143995e-03 -5.01055880775e-01 
	 -1.00000000000e+00  1.00000000000e+00  4.99167040806e-01 -1.14989820368e-03 
	 -5.00855404310e-01 -1.00000000000e+00  1.00000000000e+00  4.98995157466e-01 
	 -1.35364276316e-03 -5.01004017476e-01 -1.00000000000e+00  1.00000000000e+00 
	 4.99162855851e-01 -1.17165448681e-03 -5.00883934398e-01 -1.00000000000e+00 
	 1.00000000000e+00  4.99179372263e-01 -1.12370646189e-03 -5.00829852537e-01 
	 -1.00000000000e+00  1.00000000000e+00  4.99136370194e-01 -1.17149083705e-03 
	 -5.00857294866e-01 -1.00000000000e+00  1.00000000000e+00  4.99141819400e-01 
	 -1.17179243657e-03 -5.00863175995e-01 -1.00000000000e+00  1.00000000000e+00 
	 4.99169361526e-01 -1.12364288315e-03 -5.00819787800e-01 -1.00000000000e+00 
	 1.00000000000e+00  4.99115337432e-01 -1.17163065286e-03 -5.00836541282e-01 
	 -1.00000000000e+00  1.00000000000e+00  4.98995313180e-01 -1.35364630395e-03 
	 -5.01004168152e-01 -1.00000000000e+00  1.00000000000e+00  4.99143990496e-01 
	 -1.14978061865e-03 -5.00832256154e-01 -1.00000000000e+00  1.00000000000e+00 
	 4.98944180521e-01 -1.35322033669e-03 -5.00952535049e-01 -1.00000000000e+00 
	 1.00000000000e+00  1.00000000000e+00  1.00000000000e+00  1.00000000000e+00 
	 1.00000000000e+00  1.00000000000e+00  1.00000000000e+00  1.00000000000e+00 
	 1.00000000000e+00  1.00000000000e+00  1.00000000000e+00  1.00000000000e+00 
	 1.00000000000e+00  1.00000000000e+00  1.00000000000e+00  3.32195788482e-01 
	 3.32324998184e-01  3.32349795959e-01  3.32301650174e-01  3.32158703104e-01 
	 3.32349814927e-01  3.32364112814e-01  3.32368445545e-01  3.32352802351e-01 
	 3.32332060615e-01  3.32158765085e-01  3.32302516107e-01  3.32332074326e-01 
	 3.32279188516e-01  3.32121851092e-01 -3.34545158912e-01 -3.34386934524e-01 
	 -3.34334165644e-01 -3.34363959257e-01 -3.34507406918e-01 -3.34334181333e-01 
	 -3.34313354871e-01 -3.34297617805e-01 -3.34301887217e-01 -3.34316271485e-01 
	 -3.34507477148e-01 -3.34364157915e-01 -3.34316290599e-01 -3.34341200954e-01 
	 -3.34469886144e-01 -1.00000000000e+00 -1.00000000000e+00 -1.00000000000e+00 
	 -1.00000000000e+00 -1.00000000000e+00 -1.00000000000e+00 -1.00000000000e+00 
	 -1.00000000000e+00 -1.00000000000e+00 -1.00000000000e+00 -1.00000000000e+00 
	 -1.00000000000e+00 -1.00000000000e+00 -1.00000000000e+00 -1.00000000000e+00 
