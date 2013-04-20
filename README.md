## phantomjs-linux-armv6l

PhantomJS 1.9, compiled on Raspberry PI (Raspbian "wheezy").

PhantomJS is a headless WebKit with JavaScript API. It has fast and native support for various web standards: DOM handling, CSS selector, JSON, Canvas, and SVG. (http://phantomjs.org).


### Installation on Raspberry PI

Download the archive and extract the binary:

<pre>
$ cd /tmp
$ wget https://github.com/aeberhardo/phantomjs-linux-armv6l/archive/master.zip
$ unzip master.zip
$ cd phantomjs-linux-armv6l-master
$ bunzip2 *.bz2 && tar xf *.tar
</pre>

The binary <code>phantomjs</code> is located in the <code>bin</code> directory:

<pre>
$ ./phantomjs-1.9.0-linux-armv6l/bin/phantomjs --version
1.9.0
</pre>


I achieved the best screenshot results with the following font configuration.

__Caution:__ The following steps could mess up the font configuration for other applications!

<pre>
$ cd /usr/share
$ sudo mv fonts fonts.bak
$ sudo mkdir fonts

$ sudo apt-get install --reinstall ttf-mscorefonts-installer

$ sudo rm /usr/share/fonts/truetype/msttcorefonts/andalemo.ttf
$ sudo rm /usr/share/fonts/truetype/msttcorefonts/Andale_Mono.ttf

$ sudo fc-cache -rv
</pre>


### Build Process

PhantomJS has been built using the process described below.

__1.__ According to http://phantomjs.org/build.html :

<pre>
$ sudo apt-get update
$ sudo apt-get install build-essential chrpath git-core libssl-dev libfontconfig1-dev
$ git clone git://github.com/ariya/phantomjs.git
$ cd phantomjs
$ git checkout 1.9
</pre>


__2.__ Download additional 3rdparty files:

<pre>
$ mkdir src/qt/src/3rdparty/pixman && pushd src/qt/src/3rdparty/pixman && curl -O http://qt.gitorious.org/qt/qt/blobs/raw/4.8/src/3rdparty/pixman/README && curl -O http://qt.gitorious.org/qt/qt/blobs/raw/4.8/src/3rdparty/pixman/pixman-arm-neon-asm.h && curl -O http://qt.gitorious.org/qt/qt/blobs/raw/4.8/src/3rdparty/pixman/pixman-arm-neon-asm.S; popd
</pre>


__3.__ Open <code>./build.sh</code> and delete lines 11-34:

<pre>
.. ...
11 if [[ "$MAKEFLAGS" != "" ]]; then
12 MAKEFLAGS_JOBS=$(echo $MAKEFLAGS | egrep -o '\-j[0-9]+' | egrep -o '[0-9]+')
.. ...
34 fi
.. ...
</pre>


__4.__ Open <code>./src/qt/preconfig.sh</code> and add the option <code>' -no-pch'</code> to <code>QT_CFG</code> after <code>' -no-stl'</code>:

<pre>
.. ...
29 QT_CFG+=' -no-exceptions'       # Don't use C++ exception
30 QT_CFG+=' -no-stl'              # No need for STL compatibility
31 QT_CFG+=' -no-pch'
.. ...
</pre>


__5.__ Start compilation:

<pre>
$ nohup ./build.sh --confirm > build.sh.out 2> build.sh.err &
</pre>


__6.__ Create tarball according to http://phantomjs.org/build.html :

<pre>
./deploy/package.sh
</pre>


__7.__ The tarball can be found in the <code>./deploy</code> directory.

