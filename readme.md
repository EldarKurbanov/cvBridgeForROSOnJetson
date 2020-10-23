This uses in ROS Melodic nodes wroten on Python3 with video transferring. 

## Build cv_bridge:

```bash
sudo apt-get install python-catkin-tools python3-dev python3-numpy
sudo -H pip3 install catkin_pkg

cd ~/
git clone https://github.com/EldarKurbanov/cvBridgeForROSOnJetson
cd cvBridgeForROSOnJetson
catkin config -DPYTHON_EXECUTABLE=/usr/bin/python3 -DPYTHON_INCLUDE_DIR=/usr/include/python3.6m -DPYTHON_LIBRARY=/usr/lib/aarch64-linux-gnu/libpython3.6m.so
catkin config --install
catkin build cv_bridge
echo "source ~/cvBridgeForROSOnJetson/install/setup.bash --extend" >> ~/.bashrc
source ~/.bashrc

```

## What changed in cv_bridge for Jetson compatibility:

I was able to successfully compile cv_bridge with opencv4 below are the rough notes of what i did:

1) Added set (CMAKE_CXX_STANDARD 11) at src/vision_opencv/cv_bridge in the top level cmake
2) Changed find_package(OpenCV 3 REQUIRED TO find_package(OpenCV 4 REQUIRED on line 18 at src/vision_opencv/cv_bridge
3) In cv_bridge/src CMakeLists.txt line 35 changed to if (OpenCV_VERSION_MAJOR VERSION_EQUAL 4)
4) In cv_bridge/src/module_opencv3.cpp changed signature of two functions    

a) UMatData* allocate(int dims0, const int* sizes, int type, void* data, size_t* step, int flags, UMatUsageFlags usageFlags) const
   to
   UMatData* allocate(int dims0, const int* sizes, int type, void* data, size_t* step, AccessFlag flags, UMatUsageFlags usageFlags) const
    
b) bool allocate(UMatData* u, int accessFlags, UMatUsageFlags usageFlags) const
   to
   bool allocate(UMatData* u, AccessFlag accessFlags, UMatUsageFlags usageFlags) const
