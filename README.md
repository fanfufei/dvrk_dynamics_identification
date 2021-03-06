# A dynamic model identification package for the da Vinci Research Kit (under development)

There are parallelograms, springs, tendon couplings, cables, and counterweight in da Vinci Research Kit (dVRK)
so that we can not use existing tools to identify the dynamic parameters of it.
This software framework was developed to solve these problems.
Although this package was initially developed for the dVRK, it is also very easy to use it to identify the dynamic
parameters of other robots.


## Procedure

<p align="center">
  <img src="design/workflow.png" width="500" title="hover text">
</p>

## Features
### Finished
* Symbolic dynamic modelling based on DH parameters and geometric tree
    * Geometrical modelling
    * Dynamic modelling
    * Base parameter generation using QR decomposition
* Optimal excitation trajectory generation based on fourier-series
* Excitation of robots, using [dVRK ROS stack](https://github.com/jhu-dvrk/dvrk-ros)
* Data processing
    * Derive velocity and acceleration from position measurements
    * Zero-phase low-pass filter for original data
    * Remove the data whose velocity is close to zero to decrease the noise for Coulomb friction
* Identification
    * Joint cable torque identification
    * Ordinary Least Square (OLS)
    * Weighted Least Square (WLS)
    * Convex Optimization

* Output of the identified parameters to json files

## Requirements
* Python 2.7
* Python modules
    * NumPy, SciPy, SymPy, CvxOpt, Matplotlib, PyOpt, cloudpickle


Anaconda is recommended.

## Reference paper
A paper describing the modeling and identification of the dVRK using this package can be downloaded here.

[A convex optimization-based dynamic model identification package for the da Vinci Research Kit](https://ieeexplore.ieee.org/document/8758871)


The bibtex code for including this citation is provided:
~~~
@ARTICLE{8758871, 
author={Y. {Wang} and R. {Gondokaryono} and A. {Munawar} and G. S. {Fischer}}, 
journal={IEEE Robotics and Automation Letters}, 
title={A Convex Optimization-Based Dynamic Model Identification Package for the da Vinci Research Kit}, 
year={2019}, 
volume={4}, 
number={4}, 
pages={3657-3664}, 
keywords={Manipulator dynamics;Kinematics;Dynamics;Open source software;Couplings;Surgical Robotics: Laparoscopy;Dynamics;Calibration and Identification}, 
doi={10.1109/LRA.2019.2927947}, 
ISSN={2377-3766}, 
month={Oct},}
~~~

## Examples
* Master Tool Manipulator (MTM)
  * [IPYNB file](main_mtm.ipynb)
  * [Data collection video](https://www.youtube.com/watch?v=Sr3Uku1zgDw&index=5&list=PLkjUJbdG0RkTyNv7_gz-2ws8oqGo_xPtc&t=0s)

* Patient Side Manipulator (PSM)
  * [IPYNB file](main_psm.ipynb)

## Author
Yan Wang and Radian Gondokaryono, from [WPI AIM Lab](http://aimlab.wpi.edu/) 

## Other similar packages
When developing this work, we referred a lot from the following places:
* [SymPyBotics](https://github.com/cdsousa/SymPyBotics)
* [FloBaRoID](https://github.com/kjyv/FloBaRoID)

## Some problems
When I was using PyOpt, I found some problems with it. In ```pySLSQP.py``` file, these changes should be made to make it work.
```
gg = numpy.zeros([la], numpy.float) ==> gg = numpy.zeros(la, numpy.float)

dg = numpy.zeros([la,n+1], numpy.float) ==> dg = numpy.zeros([la[0], n + 1], numpy.float)

w = numpy.zeros([lw], numpy.float) ==> w = numpy.zeros(lw, numpy.float)

jw = numpy.zeros([ljw], numpy.intc) ==> jw = numpy.zeros(ljw, numpy.intc)
```
Or simply replace the ```pySOLVOPT.py``` file in ```anaconda2/lib/python2.7/site-packages/pyOpt/pySOLVOPT/``` with the file in ```dyn_ident_py/design``` of this repository.

		
