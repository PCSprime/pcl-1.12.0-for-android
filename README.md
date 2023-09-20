# pcl-1.12.0-for-android

#### 环境

操作系统：Ubantu18.04.6 LTS

Python：3.6.9

cmake：3.10.2

conan：1.52.0

GCC：7.5.0

git：2.17.1

#### 安装所需环境

```bash
sudo apt update
sudo apt install cmake git make python3-pip
sudo apt install clang
sudo pip3 install conan==1.52 -i https://pypi.tuna.tsinghua.edu.cn/simple
注意，conan不能安装2.x版本
安装ninja
sudo apt install ninja-build
git clone https://github.com/ninja-build/ninja.git   
cd ninja
./configure.py --bootstrap
sudo install ninja /usr/local/bin/
pip3 install ninja -i https://pypi.tuna.tsinghua.edu.cn/simple
```

由于conan默认的仓库https://conan.bintray.com实效，因此，要确保conan使用的版本较新，并导入新的根证书，才可以正常使用conan仓库。

#### 安装最新的根证书

```bash
conan config install https://github.com/conan-io/conanclientcert.git
```

#### 设置conan仓库

```
conan remote add conan-center https://center.conan.io
conan remote list #查看设置是否成功
conan-center: https://center.conan.io [Verify SSL: True]#查询到的仓库地址
```

#### 拉取项目

```bash
git clone https://github.com/bashbug/pcl-for-android.git
```

#### 修改NDK版本

##### 修改conan-profiles中的ndk版本

 修改conan-profile下面的文件，把[build-requires]下面的android-toolchain/r20@bashbug/stable修改为android-toolchain/r21@bashbug/stable即可。

##### 修改android-toolchain中的ndk版本

 打开conanfiles/android-toolchain/conanfile.py，修改version = "r20"为version = “r21”

#### 修改boost版本及源码下载地址

打开conanfiles/boost/conanfile.py，修改source(self)方法的内容如下：

```bash
tools.get("https://boostorg.jfrog.io/artifactory/main/release/{}/source/{}.tar.gz".format(self.version, self.folder_name))
```

修改boost版本的版本为1.76.0

```bash
version = "1.76.0"
```

#### 修改lz4的版本

原本的lz4版本是1.9.1，在conan仓库中已经不支持该版本了，因此需要修改为1.9.2版本。需要修改2个文件，一个是lz4的conan文件，一个是flann的文件，因为flann中依赖lz4。

打开conanfiles/flann/conanfile.py，修改requirements部分的lz4版本号，修改后的结果如下：

```bash
self.requires("lz4/1.9.2@bashbug/stable")
```

打开conanfiles/lz4/conanfile.py，修改其中的version值为1.9.2，修改后的结果如下：

```bash
version = "1.9.2"
```

#### 修改PCL配置文件

修改pcl的版本为1.12.0

```bash
version = "1.12.0"
```

在def _configure_cmake(self)做相关修改，在cmake.definitions["FLANN_USE_STATIC"] = "ON"下增添cmake.definitions["FLANN_LIBRARY_TYPE"] = "STATIC"，并且对FLANN_LIBRARIES和FLANN_LIBRARY的参数做出修改，修改为：

```bash
 "{}/liblz4.a".format(self.deps_cpp_info["flann"].lib_paths[0])
 "{}/liblz4.a".format(self.deps_cpp_info["flann"].lib_paths[0])
```

修改pcl所依赖的boost、falnn和eigen版本，修改后的结果如下：

```bash
def requirements(self):
	self.requires("boost/1.76.0@bashbug/stable")
	self.requires("flann/1.9.1@bashbug/stable") 
	self.requires("eigen/3.3.9")
```

对def source(self)做修改，注释掉tools.patch的两条语句，修改结果如下：

```
#tools.patch(base_path=self.name, patch_file="no_except.patch") 
#tools.patch(base_path=self.name, patch_file="pcl_binaries.patch")
```



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
