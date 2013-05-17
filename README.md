boost_1_53_0_xcode462_universal
===============================

Patch files to enable you to compile universal binary libraries with clang, gcc under xcode 4.6.2.<br/>
This modification is not tested completely yet.<br/>
Only compile test was done.<br/>

I'm glad if someone tests everything and posts this to boost's official.<br/>



### apply three patches
### from https://svn.boost.org/trac/boost/ticket/8266
mv [patch-libs-context-130308-0.diff] boost_1_53_0/.

### from http://github.com/toolbits/boost_1_53_0_xcode462_universal
mv [Jamfile.v2] boost_1_53_0/.

### from http://github.com/toolbits/boost_1_53_0_xcode462_universal
mv [clang-darwin.jam] boost_1_53_0/.

cd boost_1_53_0

patch -up0 < patch-libs-context-130308-0.diff<br/>
mv Jamfile.v2 libs/context/build/.<br/>
mv clang-darwin.jam tools/build/v2/tools/.<br/>



### boost with clang
sudo rm -rf /usr/local/boost_clang

./bootstrap.sh --with-toolset=clang --prefix=/usr/local/boost_clang<br/>
./b2 toolset=clang architecture=x86 address-model=32_64 cxxflags="-std=c++11 -stdlib=libc++" linkflags="-stdlib=libc++" threading=multi pch=off<br/>
sudo ./b2 install toolset=clang architecture=x86 address-model=32_64 cxxflags="-std=c++11 -stdlib=libc++" linkflags="-stdlib=libc++" threading=multi pch=off<br/>

sudo mkdir /usr/local/boost_clang/lib/a<br/>
sudo mkdir /usr/local/boost_clang/lib/dylib<br/>
sudo mv /usr/local/boost_clang/lib/*.a /usr/local/boost_clang/lib/a/.<br/>
sudo mv /usr/local/boost_clang/lib/*.dylib /usr/local/boost_clang/lib/dylib/.<br/>



### boost with gcc
sudo rm -rf /usr/local/boost_gcc

./bootstrap.sh --with-toolset=darwin --prefix=/usr/local/boost_gcc<br/>
./b2 toolset=darwin architecture=x86 address-model=32_64 threading=multi pch=off<br/>
sudo ./b2 install toolset=darwin architecture=x86 address-model=32_64 threading=multi pch=off<br/>

sudo mkdir /usr/local/boost_gcc/lib/a<br/>
sudo mkdir /usr/local/boost_gcc/lib/dylib<br/>
sudo mv /usr/local/boost_gcc/lib/*.a /usr/local/boost_gcc/lib/a/.<br/>
sudo mv /usr/local/boost_gcc/lib/*.dylib /usr/local/boost_gcc/lib/dylib/.<br/>
