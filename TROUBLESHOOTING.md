# Troubleshooting 

This page contains a small collections of issues/errors that may be experienced along with their fixes. 

**FIRST OF ALL**: did you read the main [README](./README.md) page? did you use the provided **INSTALL SCRIPTS**? If not then go back on the [README](./README.md) page, read the few lines in the install section and launch the **REQUIRED** install script. The install scripts were created in order to perform all the required install operations for you and make the install process itself as painless as possible.   


### Bad tracking performances

If you experience bad tracking performances, go in [parameters.py](./parameters.py) and set
```
kTrackingWaitForLocalMappingToGetIdle=True
```
Due to the multi-threading system (tracking thread + local mapping thread) and the non-real-time peformances of the python implementations, bad tracking performances may occur and vary according to your machine computation capabilities. 

### SIFT or SURF error

This is already explained in the main [README](./README.md) file. 

In order to use [non-free OpenCV modules](https://stackoverflow.com/questions/50467696/pycharm-installation-of-non-free-opencv-modules-for-operations-like-sift-surf) (i.e. **SIFT** and **SURF**), you need `opencv-contrib-python`. This package can be installed by running     

~~`$ pip3 install opencv-contrib-python==3.4.0.12`~~
```
$ pip3 uninstall opencv-contrib-python
$ pip3 install opencv-contrib-python==3.4.2.16
```

For a more advanced OpenCV installation procedure, you can take a look [here](https://www.pyimagesearch.com/opencv-tutorials-resources-guides/). 

How to check your installed OpenCV version:
```
$ python3 -c "import cv2; print(cv2.__version__)"

```
### g2o Error

If you run into the following error
```
 File ".../pyslam/optimizer_g2o.py", line 96, in optimization
    edge.fx = f.fx 
AttributeError: 'g2o.EdgeSE3ProjectXYZ' object has no attribute 'fx'
```
that's because you did not run the script
```
$ ./install_thirdparty.sh   
```
as explained in the main [README](./README.md) file. That's required in order to properly build and install the required thirdparty libs. 

### Problems with ROS and OpenCV

If you have ROS installed in your system and got the following error:
```
ImportError: /opt/ros/kinetic/lib/python2.7/dist-packages/cv2.so:  
undefined symbol: PyCObject_Type
```
you can run the following command in your shell: 
```
$ export PYTHONPATH=""
```
this will remove the ROS OpenCV python modules from your python path and will solve the issue. 


### Could not import PILLOW_VERSION from PIL 

Or **ImportError: cannot import name 'PILLOW_VERSION'**

If you get this error, it is very likely that pillow 7.0.0 has some troubles with pytorch. In order to solve this error, run

```
$ pip3 uninstall pillow 
$ pip3 install pillow==6.2.2
```
(fix from this [page](https://stackoverflow.com/questions/59659146/could-not-import-pillow-version-from-pil))


### ValueError: ndarray is not C-contiguous

If the following error pops-up:
```
ValueError: ndarray is not C-contiguous
```
Open pyslam/search_points.py, find this line (should be line 79):

```
projs = f_cur.project_map_points(points)
```
and replace it with :
```
projs = f_cur.project_map_points(points)
projs = projs.copy(order='C')
```

(thanks to [naughtyStark](https://github.com/naughtyStark))

### Error: python3: malloc.c:2401: sysmalloc: Assertion `(old_top == initial_top (av) && old_size == 0) ...

I got this error after messing up with the installation of different python packages related to torch and torchvision. The result was that tfeat was generating segmentation faults. In order to check if this is actually your case, run
```
$ cd test/cv     # this is compulsory
$ python3 test_tfeat.py
```
and check if you get a segmenation fault. If this is the case, try to run 
```
$ pip3 uninstall torch torchvision 
$ pip3 install torch torchvision 
```
in order to get a clean installation of the torch packages. 



### Python version

Installation issues may happen if multiple python versions are mixed. All the instructions reported in this repository assume you are using python3. If you really want to install things manually instead of using the install scripts, follow the same steps of the install scripts and be sure to use pip3 instead of pip, and good luck!