# Run FHEW

## Install FFTW

### Basic

site：http://www.fftw.org/

```bash
 tar -zxvf fftw-x.x.x.tar.gz
 cd fftw-x.x.x
./configure --prefix=/usr/local --disable-fortran --enable-shared=yes
```

参数解释：

| 参数                     | 含义                          |
| ------------------------ | ----------------------------- |
| --prefix=/usr/local/fftw | 设置安装目录为/usr/local/fftw |
| --disable-fortran        | 不包含Fortran调用的机制       |
| --enable-shared=yes      | 生成动态库.so文件             |

```bash
sudo make
sudo make install
```

### Optional: float

```bash
./configure --prefix=/usr/local --disable-fortran  --enable-float --enable-shared=yes
sudo make
sudo make install
```

| 参数           | 含义                           |
| -------------- | ------------------------------ |
| --enable-float | 生成单精度计算的头文件和库文件 |

## FHEW

```bash
git clone https://github.com/lducas/FHEW.git
cd FHEW
make
./fhewTest 1
```

出现报错：

```bash
./fhewTest: error while loading shared libraries: libfftw3.so.3: cannot open shared object file: No such file or directory
```

需要更新动态库链接：

```bash
sudo ldconfig
```

再次测试，成功

```bash
./fhewTest 1
```

```
Setting up FHEW 
Generating secret key ...  Done.
Generating evaluation key ... this may take a while ...  Done.

Testing depth-2 homomorphic circuits 1 times.
Circuit shape : (a GATE NOT(b)) GATE (c GATE d)

       NOT      Enc(1) = Enc(0)
Enc(1) AND      Enc(0) = Enc(0)
Enc(1) NOR      Enc(1) = Enc(0)
Enc(0) OR       Enc(0) = Enc(0)


Passed all tests!
```

