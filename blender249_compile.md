Compile Blender 2.49b with sgug-rse - The Tutorial
==================================================
Few days ago, I have some free times to do stuff, so I decided to try compile Blender 2.49b with sgug-rse.

This would be good starting point in case anyone want to port newer Blender to IRIX, like Blender 2.76b
(the last version to support OpenGL 1.x) or Blender 2.79b by patch it to use OpenGL 1.x.

It took me sometimes with a lots of trial and errors as there aren't any documentations about compiling Blender on IRIX anywhere I have search.

The result is it compile and link without error and the binary works too! \OwO/

Here is the step I took to compile Blender 2.49b with sgug-rse, the compile step I based from [blender249 AUR](https://aur.archlinux.org/cgit/aur.git/?h=blender249/)

I assume you know a bit on how to compile stuff from source.

Install sgug-rse
----------------

Install sgug-rse-0.0.7beta on your SGI using given instructions from [https://github.com/sgidevnet/sgug-rse](https://github.com/sgidevnet/sgug-rse), then setup [RSE Cloud Repo](https://github.com/sgidevnet/sgug-rse/wiki#add-the-cloud-repo).

After finished install, install these packages.

```bash
sudo tdnf install gcc gcc-c++ git autoconf automake cmake make python2-devel freetype-devel gettext-devel libiconv-devel libjpeg-turbo-devel libpng-devel libtiff-devel zlib-devel OpenEXR-devel
```

Due to sgug-rse's `glxtokens.h` didn't have some of SGI's GLX defines, it will result in `GLX_HYPERPIPE_PIPE_NAME_LENGTH_SGIX` compile error.

Remove sgug-rse's `glxtokens.h`.

```bash
sudo rm /usr/sgug/include/GL/glxtokens.h
```

To use sgug-rse, run this command in the terminal

```bash
/usr/sgug/bin/sgugshell
```

Compile and install libSDL
--------------------------
I'm using SDL 1.2.15 from https://github.com/libsdl-org/SDL-1.2/

```bash
cd SDL-1.2
./autogen.sh
./configure
make && make install
```

Compile and install OpenAL
--------------------------

I'm using source code from Nekoware since it was already available on my Tezro

```bash
cd openal-1.1/linux
./autogen.sh
./configure
make && make install
```

Prepairing Blender 2.49b source code
------------------------------------

Use the already patched code from my github repo [https://github.com/kytulendu/blender-2.49b](https://github.com/kytulendu/blender-2.49b)

Compile Blender 2.49b
---------------------

```bash
cd blender-2.49b

mkdir build && cd build

# change to -march=mips3 or -march=mips4 to match your machine
cmake \
    -DCMAKE_EXE_LINKER_FLAGS:STRING="-lmoviefile -ldmedia -lX11 -lGL -lGLcore -lGLU -ldl -lpthread -Wl,-rpath-link=/usr/lib32 -Wl,-rpath=/usr/lib32:/usr/sgug/lib32 -Wl,--allow-shlib-undefined" \
    -DCMAKE_CXX_FLAGS:STRING="-O2 -march=mips3 -g" \
    -DCMAKE_C_FLAGS:STRING="-O2 -march=mips3 -g" \
    -DPYTHON_EXECUTABLE:PATH=/usr/sgug/bin/python2 \
    -DPYTHON_LIBRARY:PATH=/usr/sgug/lib32/libpython2.7.so \
    -DPYTHON_INCLUDE_DIR:PATH=/usr/sgug/include/python2.7 \
    -DFREETYPE_INC:PATH=/usr/sgug/include/freetype2 \
    -DFREETYPE_LIB:PATH=/usr/sgug/lib32/libfreetype.so.6 \
    -DOPENEXR_INC:PATH=/usr/sgug/include/OpenEXR \
    -DWITH_OPENMP:BOOL=ON \
    -DWITH_OPENJPEG:BOOL=ON \
    -DWITH_OPENEXR:BOOL=ON \
    -DWITH_PLAYER:BOOL=ON \
    ../

# use "make -j 2" to use 2 cpus to compile blender
# use "ninja" if you add "-GNinja" flag to cmake
make
```

no errors yay!!

![Compiled](./resources/blender249_compiled.jpg)

```bash
# build plugins
cp -r ../release/plugins ./bin
mkdir -p ./bin/plugins/include
cp ../source/blender/blenpluginapi/*.h ./bin/plugins/include
chmod +x ./bin/plugins/bmake

# edit ./bin/plugins/bmake
# copy CC, CFLAGS, LD, LDFLAGS from Linux portion to IRIX64

make -C ./bin/plugins
```

And run blender.

```bash
# run blender
cd bin
./blender
```

Success!!!!

![Running](./resources/blender249_running.jpg)

Benchmark
---------

Using test.blend from [Ian's SGI Depot](http://www.sgidepot.co.uk/blender.html) on SGI Tezro R16000 800Mhz X4 4MB L2 Cache, 3GB RAM, IRIX 6.5.22

1 render threads
- official Blender 2.49a                        06:22.55
- self build Blender 2.49b (-O2 -march=mips3)   06:34.49
- self build Blender 2.49b (-O2 -march=mips4)   06:21.90

4 render threads
- official Blender 2.49a                        01:54.97
- self build Blender 2.49b (-O2 -march=mips3)   01:58.50
- self build Blender 2.49b (-O2 -march=mips4)   01:53.03

8 render threads
- official Blender 2.49a                        01:56.86
- self build Blender 2.49b (-O2 -march=mips3)   01:51.57
- self build Blender 2.49b (-O2 -march=mips4)   01:48.75
