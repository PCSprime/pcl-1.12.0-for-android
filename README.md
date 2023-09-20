# pcl-1.12.0-for-android

#### 环境

操作系统：Ubantu18.04.6 LTS

Python：3.6.9

cmake：3.10.2

conan：1.52.0

GCC：7.5.0

git：2.17.1



经过一次编译后，报错后，找到pcl的cmakelists.txt文件，在/.conan/data/pcl/1.12.0/bashbug/stable/source/pcl/目录下
```
# FLANN (required)
find_package(FLANN 1.9.1 REQUIRED)
if(NOT (${FLANN_LIBRARY_TYPE} MATCHES ${PCL_FLANN_REQUIRED_TYPE}) AND NOT (${PCL_FLANN_REQUIRED_TYPE} MATCHES "DONTCARE"))
  message(FATAL_ERROR "Flann was selected with ${PCL_FLANN_REQUIRED_TYPE} but found as ${FLANN_LIBRARY_TYPE}")
endif()
```

在endif()后添加

link_libraries(/home/用户/.conan/data/flann/1.9.1/bashbug/stable/package/xxx/lib/libflann_cpp_s.a)

其中xxx为类似0ae23ce156a999016ff3644328f7150befab40a6的目录

#### 编译

##### armeabi-v7a

```bash
./pcl-build-for-android.sh armeabi-v7a
```

##### arm64-v8a

```bash
./pcl-build-for-android.sh arm64-v8a
```
