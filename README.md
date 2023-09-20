# pcl-1.12.0-for-android

#### 环境

操作系统：Ubantu18.04.6 LTS

Python：3.6.9

cmake：3.10.2

conan：1.52.0

GCC：7.5.0

git：2.17.1



经过一次编译后，报错后，找到pcl的cmakelists.txt文件，在/.conan/data/pcl/1.12.0/bashbug/stable/source/pcl/目录下![image-20230920141121882](C:\Users\彭李想\AppData\Roaming\Typora\typora-user-images\image-20230920141121882.png)

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
