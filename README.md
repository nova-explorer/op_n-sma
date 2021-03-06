# Order Parameter for Liquid Crystal Polymers in Nematic and Smectic A mesophases

current version : 1.1

# Known bugs
* Script fails if terminal is too small to draw menu. Use a terminal of minimum size 18x97. Check terminal size with stty size
* Script cannot draw menu with mpi. Made so it only draws it if 1 processor is used
* For boolean options, anything but False will give True since its converted from string

# Description
The script works with LAMMPS trajectory files and it calculates orientational order parameter as well as orientational and positional order parameter (along z axis).
#CHECK
It was designed for nematic and smectic A mesophases of liquid crystal polymers modelised as beads (lennard-jonnes) for chains and ellipsoids(gay-berne) for liquid crystalline units. Example of the system can be found in the given trajectory files.
It was forked from Etienne Cuierrier's repository, small changes were made and a ui was added to allow the options to be changed more easily.

# Dependencies
Python 2.7 and following modules:
* collections
* curses
* glob
* gzip
* matplotlib
* mpi4py
* numpy
* pandas
* scipy

So far no bugs we found while using with python3 but they were designed on python2.

# Use
It is recommended to put the master directory in PATH if you want to run on many datasets.
Start by executing op_main.py in your working directory (where trajectories are). You will see a menu appear where you can choose your options. The grey status bar at the bottom will give you information about using the menu. Press <Enter> 2 times (eg. input nothing) to get a quick description of the option.

More elaborate descriptions can be found in op_traj_main.py and are pasted here for reference.
    * nprocs(int): number of processor (read from mpirun command)
    * rank(int): rank of the process
    * first_frame(int): the first frame of the trajectory
    * last_frame(int): the last frame of the trajectory
    * wrap(bool): True if the coordinates are to be wrapped
    * visualize(bool): True if the gz12 in function of z12 graph for the SmA OP is desired(3X slower)
    * ini_layer_spacing(float): SmA layer spacing to optimize
    * gb_type(int): atom type of the ellipsoid
    * gb_ends_type(int): atom type of the pseudo-atoms at the end of the ellipsoids
    * atoms_per_monomer(int): atoms per monomer in the chains
    * number_of_monomer(int): total number of monomer in the system
    * number_of_chains(int): number of polymer chains in the system
    * file_pattern(str): pattern of the files you want to scan. Be as specific as possible.

# Trajectory files
Example of the trajectory files used are given for reference in example/. They are generated by this LAMMPS command:
	
	dump 1 all custom/gz 100000 ellipsoid.*.dump.gz id type xu yu zu &
	c_orient[1] c_orient[2] c_orient[3] c_orient[4] &
	c_shape[1] c_shape[2] c_shape[3]
	dump_modify 1 sort id
As of now, there are no plans to make script accept more formats of trajectory files.

# Output
gz12 graphics are saved in png files and op data are saved in text files. The structure of those files are as such:
* nematic_OP.out :
	timestep	nematic OP	director x	director y	director z
* sma_OP.out :
	timestep	sma OP		optimized layer spacing

# Todo list
* fix known bugs
* op_traj_mpi.py -> a way to rename output files
* more graphics maybe
* dump_dataframe.py -> multiple steps per dump
* nematic_sma_OP -> orientation from quaternion
* nematic_sma_OP -> multiple ellipsoid type
* nematic_sma_OP -> automatic reading of atoms_per_monomer and number_of_monomer/number_of_chains
