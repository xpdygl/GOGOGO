# ubuntu安装protoc全过程



```
git clone https://github.com/protocolbuffers/protobuf.git
sudo apt-get install autoconf automake libtool curl make g++ unzip
cd protobuf/
git submodule update --init --recursive
sudo ./autogen.sh   #生成配置脚本
sudo ./configure    #生成Makefile文件，为下一步的编译做准备，可以加上安装路径：--prefix=path ，默认路径为/usr/local/
sudo make           #从Makefile读取指令，然后编译
sudo make check     #可能会报错，但是不影响,对于安装流程没有实质性用处，可以跳过该步
sudo make install 
sudo ldconfig       #更新共享库缓存
which protoc        #查看软件的安装位置
protoc --version    #检查是否安装成功


```

