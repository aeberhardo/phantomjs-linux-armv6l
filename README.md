phantomjs-linux-armv6l
======================

PhantomJS 1.8.1, compiled on Raspberry PI (Raspbian "wheezy").

PhantomJS: http://phantomjs.org



According to http://phantomjs.org/build.html:

$ sudo apt-get install build-essential chrpath git-core libssl-dev libfontconfig1-dev
$ cd phantomjs
$ git checkout 1.8


Download additional 3rdparty files:

$ mkdir src/qt/src/3rdparty/pixman && pushd src/qt/src/3rdparty/pixman && curl -O http://qt.gitorious.org/qt/qt/blobs/raw/4.8/src/3rdparty/pixman/README && curl -O http://qt.gitorious.org/qt/qt/blobs/raw/4.8/src/3rdparty/pixman/pixman-arm-neon-asm.h && curl -O http://qt.gitorious.org/qt/qt/blobs/raw/4.8/src/3rdparty/pixman/pixman-arm-neon-asm.S; popd



Open ./build.sh and delete lines 11-31:

.. ...
11 if [[ "$MAKEFLAGS" != "" ]]; then
12 MAKEFLAGS_JOBS=$(echo $MAKEFLAGS | egrep -o '\-j[0-9]+' | egrep -o '[0-9]+')
.. ...
31 fi
.. ...

Open ./src/qt/preconfig.sh and add ' -no-pch' option to QT_CFG after ' -no-stl':

.. ...
29 QT_CFG+=' -no-exceptions'       # Don't use C++ exception
30 QT_CFG+=' -no-stl'              # No need for STL compatibility
31 QT_CFG+=' -no-pch'
.. ...


Start compilation (takes about half a day):

$ nohup ./build.sh --confirm > build.sh.out 2> build.sh.err &


Create tarball according to http://phantomjs.org/build.html:

./deploy/package.sh


The tarball can be found in the ./deploy directory.
