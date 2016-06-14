#

## llvm compilation

```sh
cd ~;
mkdir opt;
cd opt;
svn co http://llvm.org/svn/llvm-project/llvm/trunk llvm;
cd llvm/tools
svn co http://llvm.org/svn/llvm-project/cfe/trunk clang;
cd ..;
mkdir build;
cd build;
cmake -G "Unix Makefiles" -DCMAKE_BUILD_TYPE=Release ../
make j2
```