
# View-Consistent 4D Light Field Depth Estimation
 [Numair Khan](https://cs.brown.edu/~nkhan6)<sup>1</sup>, 
 [Min H. Kim](http://vclab.kaist.ac.kr/minhkim/)<sup>2</sup>,
 [James Tompkin](http://www.jamestompkin.com)<sup>1</sup><br>
 <sup>1</sup>Brown, <sup>2</sup>KAIST<br>
 BMVC 2020

### [Paper]() | [Project Homepage]() | [Presentation Video]() | [Supplemental Results Video]() 

<img src="./view-consistent-depth.gif" width="100%"><br>

## Citation
If you use this code in your work, please cite the following works:

```
@article{khan2020vclfd,
  title={View-consistent 4D Lightfield Depth Estimation},
  author={Numair Khan, Min H. Kim, James Tompkin},
  journal={British Machine Vision Conferfence},
  year={2020},
}

@techreport(khan2020falfd,
  title={Fast and Accurate {4D} Light Field Depth Estimation},
  author={Numari Khan, Min H. Kim, James Tompkin},
  year={2020},
  institution={Brown University},
  number={CS-20-01},
}
```

## Running the MATLAB Code
* [Installing ImageStack](#installing-imagestack)
* [Generating Depth](#generating-depth)
* [Troubleshooting](#troubleshooting)

### Installing ImageStack
The code uses ImageStack's implementation of Richard Szeliski's LAHBPCG solver. Along with this repo, you will also have to clone the ImageStack submodule:

```
$ git clone https://github.com/brownvc/lightfielddepth.git
$ cd lightfielddepth
$ git submodule init
```

You may have to install the FFTW3 library for ImageStack:

```
$ sudo apt-get install fftw3
```

Then compile the MEX interface to ImageStack:

```
$ matlab -nodisplay -r "compile_mex; exit"
``` 

### Generating Depth
To generate disparity estimates for all views of a light field, use `run.sh` followed by the path to the light field file:

```$ sudo ./run.sh <path-to-light-field> ```

The light field is provided as a `.mat` file containing a 5D array. The dimensions of the 5D array should be ordered as (y, x, rgb, v, u) where "rgb" denotes the color channels. 

```
                 u              
       ---------------------------->
       |  ________    ________
       | |    x   |  |    x   |
       | |        |  |        |
     v | | y      |  | y      | ....
       | |        |  |   pp     |     
       | |________|  |________| 
       |           :
       |           :
       v
```

Alternatively, a path to a directory of images may be provided to `run.sh`. The directory should contain a file called `config.txt` with the dimensions of the light field on the first line in format `y, x, v, u`.

<bf>Make sure to set the camera movement direction for both u and v in `parameters.m`.</bf>

The depth estimation results are output to a 4D MATLAB array in `./results/<time-stamp>/`.

### Troubleshooting
- Code fails with error `Index exceeds the number of array elements`: Make sure you are following the correct dimensional
ordering; for light field images this should be `(y, x, rgb, v, u)` and for depth labels `(y, x, v, u)`.
- The output has very high error: Make sure you specify the direction in which the camera moves in u and v. This can be done by setting the boolean variables `uCamMovingRight` and `vCamMovingRight` in `parameters.m`. The camera movement direction determines the occlusion order of EPI lines, and is important for edge detection and depth ordering.
- The code has been run and tested in MATLAB 2019b. Older version of MATLAB may throw errors on some functions.
