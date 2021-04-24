## Realsense T256 Tracking camera

The Intel® RealSense™ Tracking Camera T265 includes two greyscale cameras with fish-eye lens, an IMU, and an Intel® Movidius™ Myriad™ 2 VPU. All of the V‑SLAM algorithms run directly on the VPU, allowing for very low latency and extremely efficient power consumption (1.5W).

## Sensor origin and coordinate system

![image](https://user-images.githubusercontent.com/58238736/115772105-512ffa80-a3e1-11eb-8bb0-931d9d3ae6ea.png)

1. Positive X direction is towards right imager ( Roll)

2. Positive Y direction is upwards toward the top of the device (pitch)

3. Positive Z direction is inwards toward the back of the device (yaw)

   

   The center of tracking corresponds to the center location between the right and left monochrome imagers on the PCB.

   

   When T265 tracking starts, an origin coordinate system is created and RealSense SDK provides T265 poses relative to it. World's Y axis is always aligned with gravity and points to the sky. World's X and Z axes are not globally set, but determined when tracking starts based on the initial orientation of the T265 device with the initial -Z world axis as the projection of the camera axis to the ground plane. This does mean that the initial yaw can seem random when the device is started in a downward facing configuration, say on the drone. All T265 (and librealsense) coordinate systems are right-handed.

   ![image](https://user-images.githubusercontent.com/58238736/115772158-5d1bbc80-a3e1-11eb-98c1-8723eef4ea29.png)

## A. Install Realsense2

Install realsense2 from binary distribution (Ubuntu 16.04 Xenial, 18.04 Bionic and 20.04 Focal)

### 1. Register the server's public key:

```python
sudo apt-key adv --keyserver keys.gnupg.net --recv-key F6E65AC044F831AC80A06380C8B3A55A6F3EFCDE || sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-key F6E65AC044F831AC80A06380C8B3A55A6F3EFCDE
```

### 2. Add the server to the list of repositories:

- Ubuntu 20.04 Focal:

```python
sudo add-apt-repository "deb http://realsense-hw-public.s3.amazonaws.com/Debian/apt-repo focal main" -u
```

- Ubuntu 18.04 Bionic:

```python
sudo add-apt-repository "deb http://realsense-hw-public.s3.amazonaws.com/Debian/apt-repo bionic main" -u
```

- Ubuntu 16.04 Xenial:

```python
sudo add-apt-repository "deb http://realsense-hw-public.s3.amazonaws.com/Debian/apt-repo xenial main" -u
```

### 3. Install packages:

```python
sudo apt install librealsense2-dkms librealsense2-utils librealsense2-dev
```

### 4. Verify installation:

- Reconnect the Intel RealSense depth camera and run: 

```python
realsense-viewer
```

![image](https://user-images.githubusercontent.com/58238736/115771763-e8488280-a3e0-11eb-89b1-c92628db14a0.png)



- result for camera T265 2D

![image](https://user-images.githubusercontent.com/58238736/115771719-df57b100-a3e0-11eb-8a6a-fa70f74712a1.png)

- result for camera T265 3D

![image](https://user-images.githubusercontent.com/58238736/115771791-f26a8100-a3e0-11eb-9462-4695cf5450ff.png)

## **B. How to Uninstall** **Realsense**2

-  Remove a single package with: 

``` python
sudo apt-get purge <package-name> 
```

## C. **How to** **create** **dataset** **from** **T265** **Realsense camera** 

We have created our own dataset with the help of Intel T265 by changing the examples given by Intel RealSense. 

We need following to create our dataset: 

- [x] Sequence of Images 

- [x] Ground Truth Data (pose) 

- [ ] Calibration file (calib.txt) 

- [ ] Time-stamp (times.txt) 



### 1. Install Realsense SDK must install realsense2 above

### 2. Upgrading the Packages

this step had function for update your installation

- Refresh the local packages cache by invoking: 

```python
sudo apt-get update 
```

- Upgrade all the installed packages, including librealsense with: 

``` python
sudo apt-get upgrade 
```

- To upgrade selected packages only a more granular approach can be applied: 

 ```python
sudo apt-get --only-upgrade install <package1 package2 ...> 
 ```

 ```python
sudo apt-get --only-upgrade install librealsense2-utils librealsense2-dkms
 ```

 ### 3. Download/ Clone Librealsense GitHub repository:

```python
git clone https://github.com/IntelRealSense/librealsense.git  
```

- Navigate to Librealsense ``root directory`` to run the following scripts. 
- Unplug any connected Intel RealSense camera. 

### 4. Install the core packages needed to build librealsense binaries and the affected kernel modules:

```python
sudo apt-get install git libssl-dev libusb-1.0-0-dev pkg-config libgtk-3-dev
```

- Distribution-specific packages on Ubuntu 18 and 20

```python
sudo apt-get install libglfw3-dev libgl1-mesa-dev libglu1-mesa-dev
```

### **5.** Replace some file 

- Get into this directory

```bash
- Librealsense
	- Example
		- trajectory
```

- Clone this repository  

```python
git clone https://github.com/aji-ptn/make-dataset-from-T256.git 
```

- Extract and replace file in trajectory 

![image-20210425022535673](/home/aji/.config/Typora/typora-user-images/image-20210425022535673.png)

- Open ``rs-trajectory.cpp`` and change ``line 502`` directory and make file ``pose.txt`` to save pose estimation of camera position. 

![image-20210425022703980](/home/aji/.config/Typora/typora-user-images/image-20210425022703980.png)

### 6. **M**ake folder build 

- Navigate to librealsense ``root directory`` again and make folder build 

```python
mkdir build && cd build
```

### 7. Run Cmake

```python
cmake ../ -DBUILD_EXAMPLES=true -DCMAKE_BUILD_TYPE=Release
```

### 8. Reinstall

- Recompile and Install Librealsense binaries: 

```python
sudo make uninstall && make clean && make –j$(nproc) && sudo make install
```

![image-20210425022948910](/home/aji/.config/Typora/typora-user-images/image-20210425022948910.png)

### 9. Run 

- Get into this directory

```bash
- Librealsense
	- build
		- Example
			- trajectory
```

- To save image run this command. 

```python
rs-trajectory
```

![image-20210425023304068](/home/aji/.config/Typora/typora-user-images/image-20210425023304068.png)

- Result for sequence

![image-20210425023341513](/home/aji/.config/Typora/typora-user-images/image-20210425023341513.png)

- Result for pose

![image-20210425023332431](/home/aji/.config/Typora/typora-user-images/image-20210425023332431.png)



## D. Reference

- https://roboticslab-uc3m.github.io/installation-guides/install-realsense2.html

- https://www.intel.com/content/dam/support/us/en/documents/emerging-technologies/intel-realsense-technology/Intel-RealSense-SDK2-Github-Guide.pdf

- https://github.com/IntelRealSense/librealsense

- https://github.com/Shivani1796/How-to-create-MOIL-Dataset



# MOIL LAB Ming Chi University of Technology

#### contact 

- Aji pamungkas Tri Nurcahyo 

- email : ajipamungkastrinurcahyo@gmail.com

- email : M09158022@o365.mcut.edu.tw