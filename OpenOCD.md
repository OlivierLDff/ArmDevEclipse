# OpenOCD

We suppose that you have a fully [MSYS2 environment set up](./MSYS2.md).

## Dependancy 

### MSYS

#### LibUSB

```bash
cd ~/dev
git clone https://github.com/libusb/libusb.git
cd libusb
./autogen.sh
make -j8
make install
cd ~/dev
```

#### LibUSB 0.1

```bash
cd ~/dev
git clone https://github.com/libusb/libusb-compat-0.1
cd libusb-compat-0.1
./autogen.sh
make -j8
make install
cd ~/dev
```

#### LibConfuse *(required by libftdi1)*

```bash
cd ~/dev
git clone https://github.com/martinh/libconfuse
cd libconfuse
./configure
./autogen.sh./
make -j8
make install
cd ~/dev
```

#### LibFtdi1 *(required for FTDI device support)*

```bash
cd ~/dev
git clone git://developer.intra2net.com/libftdi
cd libftdi
mkdir build && cd build
cmake ..
make -j8
make install
cd ~/dev
```

#### HidAPI *(required for CMSIS-DAP support)*

```bash
cd ~/dev
git clone https://github.com/signal11/hidapi
cd hidapi
./bootstrap
./configure
make -j8
make install
cd ~/dev
```

> Note: You might need to hack configure.ac script in order to compile on msys without doing cross compilation. 
>
> ```
> *-msys*)
> 	AC_MSG_RESULT([ (Windows back-end, using MinGW)])
> 	backend="windows"
> 	os="windows"
> 	threads="windows"
> 	win_implementation="mingw"
> 	;;
> ```
>
> Insert that code in AC_MSG_CHECKING (around line 53)

### MinGW-32

```
cd && mkdir dev-mingw-32
cd dev-mingw-32
pacman -S unzip bzip2 base-devel mingw-w64-x86_64-toolchain mingw-w64-x86_64-libusb mingw-w64-x86_64-libusb-compat-git mingw-w64-x86_64-hidapi mingw-w64-x86_64-libftdi
```

### MinGW-64

```
cd && mkdir dev-mingw-64
cd dev-mingw-64
pacman -S unzip bzip2 base-devel mingw-w64-i686-toolchain mingw-w64-i686-libusb mingw-w64-i686-libusb-compat-git mingw-w64-i686-hidapi mingw-w64-i686-libftdi
```

> Tested with:
>
> * libgusb 0.2.11-1
> * libusb 1.0.22-1 
> * libusb-compat-git r72.92deb38-1
> * libftdi 1.4-2
> * hidapi 0.8.0rc1-4

## Build OpenOCD

### MSYS2

```bash
git clone --recurse-submodules -j8 git://git.code.sf.net/p/openocd/code openocd
cd openocd
./bootstrap
./configure
make -j8
make install
```

You should check after configure that everything is going to be built:

* MSYS2

```
libjaylink configuration summary:
 - Package version ................ 0.2.0-git-8645845
 - Library version ................ 0:0:0
 - Installation prefix ............ /usr
 - Building on .................... x86_64-pc-msys
 - Building for ................... x86_64-pc-msys

Enabled transports:
 - USB ............................ yes
 - TCP ............................ yes



OpenOCD configuration summary
--------------------------------------------------
MPSSE mode of FTDI based devices        yes (auto)
ST-Link JTAG Programmer                 yes (auto)
TI ICDI JTAG Programmer                 yes (auto)
Keil ULINK JTAG Programmer              yes (auto)
Altera USB-Blaster II Compatible        yes (auto)
Bitbang mode of FT232R based devices    yes (auto)
Versaloon-Link JTAG Programmer          yes (auto)
TI XDS110 Debug Probe                   yes (auto)
OSBDM (JTAG only) Programmer            yes (auto)
eStick/opendous JTAG Programmer         yes (auto)
Andes JTAG Programmer                   yes (auto)
USBProg JTAG Programmer                 no
Raisonance RLink JTAG Programmer        no
Olimex ARM-JTAG-EW Programmer           no
CMSIS-DAP Compliant Debugger            yes (auto)
Cypress KitProg Programmer              yes (auto)
Altera USB-Blaster Compatible           no
ASIX Presto Adapter                     no
OpenJTAG Adapter                        no
SEGGER J-Link Programmer                yes (auto)

```

If something is no it mean that you have some library missing:

* libusb 1.0
* libusb 0.1
* libftdi1
* hidapi
* libjaylink

#### MinGW 32 bits

```
cd ~/dev-mingw-32
git clone https://github.com/kekyo/OpenOCDonMinGW
cd OpenOCDonMinGW
./build.sh
```

#### MinGW 64 bits

```
cd ~/dev-mingw-64
git clone https://github.com/kekyo/OpenOCDonMinGW
cd OpenOCDonMinGW
./build.sh

 ./configure  --enable-static   --disable-shared --disable-gccwarnings --enable-remote-bitbang --enable-internal-jimtcl  --enable-internal-libjaylink CFLAGS="-O2 -fomit-frame-pointer --static"

```

- If building finished normally, OpenOCD built binaries store into "artifacts/openocd-*_*.tar.bz2"
- ex: "artifacts/openocd-0.10.0_mingw-w64-i686.tar.bz2"

```bash
echo "# ==============================================================="
echo "# clone openocd"

# in dev/
cd ~/dev-mingw-64

git clone --recurse-submodules -j8 git://git.code.sf.net/p/openocd/code openocd
cd openocd # in openocd
mkdir build
cd build # in openocd/build

export BUILD=${BUILD:-`gcc -dumpmachine`}
export SUFFIX${SUFFIX:-`date --iso-8601`}

export OPENOCD_VERSION=${OPENOCD_VERSION:-0.10.0}
export OPENOCD=${OPENOCD:-openocd-${OPENOCD_VERSION}}

export BASE=`pwd`
export STAGE=${BASE}/stage
export ARTIFACTS=${BASE}/artifacts

export PREFIX=${STAGE}/${BUILD}/${OPENOCD}
export NPROC=${NPROC:-$((`nproc`*2))}
export PARALLEL=${PARALLEL:--j${NPROC}}

export PATH=${STAGE}:$PATH;

echo PATH : $PATH
echo BUILD : $BUILD
echo SUFFIX : $SUFFIX
echo BUILD : $BUILD
echo OPENOCD_VERSION : $OPENOCD_VERSION
echo OPENOCD : $OPENOCD
echo BASE : $BASE
echo STAGE : $STAGE
echo ARTIFACTS : $ARTIFACTS
echo PREFIX : $PREFIX
echo NPROC : $NPROC
echo PARALLEL : $PARALLEL

mkdir artifacts
cd artifacts # in openocd/build/artifacts

echo "# ==============================================================="
echo "# download libsetup api"

if [ ! -f libsetupapi.a ]; then
    wget https://github.com/msysgit/msysgit/raw/master/mingw/lib/libsetupapi.a
fi

cd .. # in openocd/build

echo "# ==============================================================="
echo "# hidapi"

rm -rf hidapi
git clone https://github.com/signal11/hidapi
cd hidapi # in openocd/build/hidapi
./bootstrap
mkdir build
cd build # in openocd/build/hidapi/build
.-/configure --prefix=${PREFIX} \
    --enable-static \
    --disable-shared \
    CFLAGS="-O2 -fomit-frame-pointer --static" \
    LDFLAGS="--static ${ARTIFACTS}/libsetupapi.a"
make ${PARALLEL}
make install

cd ../../../ # in openocd

./bootstrap
./configure --prefix=${PREFIX} \
    --enable-static \
    --disable-shared \
    --enable-verbose \
    --disable-gccwarnings \
    --enable-remote-bitbang \
    --enable-internal-jimtcl \
    --enable-internal-libjaylink \
    CFLAGS="-O0 -ggdb3 -fomit-frame-pointer --static -I${PREFIX}/include" \
    LDFLAGS="--static ${ARTIFACTS}/libsetupapi.a -L${PREFIX}/lib"
make ${PARALLEL}
make install

```



## Sources

* [OpenCDonMinGW](https://github.com/kekyo/OpenOCDonMinGW)
* [OpenOCD](http://openocd.org/doc/doxygen/html/patchguide.html)
* [Build ESPressif openOCD](https://docs.espressif.com/projects/esp-idf/en/latest/api-guides/jtag-debugging/building-openocd-windows.html)