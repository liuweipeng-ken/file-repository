---
title: caffe在mac上的配置
---
## 配置环境
　　系统：maxOS Sierra 10.12.4
　　电脑型号：2015年的13寸MacBook Pro
　　处理器：2.7 GHz Intel Core i5
　　内存：Intel Iris Graphics 6100 1536 MB
## 配置步骤
### 下载并安装配置Anaconda
　　下载地址：[点此下载](https://www.continuum.io/downloads/)  ，下载命令行安装模式，将下载来的安全文件拷贝到/usr/local/Cellar下
　　执行命令：bash Anaconda2-4.2.0-MacOSX-x86_64.sh
　　设置环境变量：export PATH=/usr/local/Cellar/anaconda2/bin:\$PATH
　　　　　　　    export DYLD\_FALLBACK\_LIBRARY_PATH=/usr/local/Cellar/anaconda2:/usr/local/lib:/usr/lib
### 批量安装相关包及依赖
　　brew install --fresh -vd snappy leveldb gflags glog szip lmdb homebrew/science/opencv
　　brew install --build-from-source --with-python --fresh -vd protobuf
　　brew install --build-from-source --fresh -vd boost boost-python
### 下载caffe源码包
　　cd 到 /usr/local/Cellar目录下
　　执行命令：git clone https://github.com/BVLC/caffe.git 

### 修改配置文件
　　按照[这里链接下的文件](https://github.com/yaoleiliu/file-repository/blob/master/Makefile.config)，按照这个文件进行修改。
### 编译
　　make all
　　for req in  \$此处左小括号cat python/requirements.txt此处右小括号; do pip install \$req; done
　　make pycaffe 
　　make distribute 
　　设置环境变量：export PYTHONPATH=/usr/local/Cellar/caffe/python 
### 测试
　　cd 到caffe目录下
　　\./data/mnist/get_mnist.sh 
　　\./examples/mnist/create_mnist.sh  
　　此时有可能出现错误：
　　dyld: Library not loaded: @rpath/libhdf5_hl.10.dylib 
　　Referenced from: /Users/work/gitclone/caffe/.build\_release/test/test_all.testbin  
　　Reason: image not found  
　　解决方法：
　　install\_name\_tool -add\_rpath '/Users/work/anaconda/lib'  /Users/work/gitclone/caffe/.build\_release
/test/test_all.testbin
　　继续执行：./examples/mnist/create_mnist.sh 
　　此时有可能出现错误：
　　Cannot use GPU in CPU-only Caffe  
　　解决方法：
　　打开文件：examples/mnist/lenet\_solver.prototxt，将solver_mode修改为CPU
　　出现如下图示，即可表示安装成功
　　![成功示例图片](https://d2ppvlu71ri8gs.cloudfront.net/items/2g0A103K1E0P0S212y2Q/109.png?v=bff5b17a)
　　    