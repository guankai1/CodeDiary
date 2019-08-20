## FFmpegLearn
### 概述
ffmpeg支持大量的音视频编解码，通常大家所说的视频格式(format)可以理解为压缩格式(container)或者混流格式(Muxing)，例如`avi`, `mp4`, `mkv`等，他们相当于一个容易，讲视频和音频、字幕包装在一起，并按照制定的方式同步输出。
编、解码格式，也就是所谓的裸流的格式(raw bitstream)，视频编码方式很多，现在使用最多的就是`H.264 (AVC)`，其他编码方式还有`MPEG-2`, `MPEG-4 Part 2`, `MPEG-4 Part10`
封装格式的区域之一在于对音视频编码格式的支持
技术点包含为一下四个: 封装技术，视频压缩编码技术，音频压缩编码技术，流媒体协议

### 播放原理
1. 解协议
2. 解封装
3. 解码
4. 音视频同步

#### 解协议
音视频在传输过程中加入了一些信令数据，如播放，暂停，等，解协议例如`HTTP`, `RMTP`, `MMS`, 后输出压缩格式的数据。

#### 解封装
例如将压缩格式例如`MP4`, `MKV`, `RMVB`, `FLV`, `AVI`输出`H.264`编码的视频码流和`AAC`格式的音频码流。

#### 解码
最复杂最重要的一环, 将`H.264(AVC)`, `MPEG2`, `VC-1`格式的视频输出为非压缩的颜色数据如`YUV420P`, `RGB`。将`AAC`, `MP3`, `AC-3`格式的音频输出为非压缩的音频采样数据，如`PCM`。视频如果不编码，会非常大

#### 封装格式
AVI, MKV几乎覆盖所有格式, 除了 AVI，其余的都可以边下边播，RMVB支持的格式少一点

|名称|推出机构|流媒体|支持的视频编码|支持的音频编码|目前使用领域|
|---|---|---|---|---|---|
|AVI   |Microsoft Inc.   |不支持   |几乎所有格式   | 几乎所有格式  |  BT下载影视 |
|MP4   |MPEG   |支持   |MPEG-2, MPEG-4, H.264, H.263等   |AAC, MPEG-1 Layers I, II, III, AC-3等   |互联网视频网站   |
|TS   |MPEG   |支持   |MPEG-1, MPEG-2, MPEG-4, H.264   |MPEG-1 Layers I, II, III, AAC,   |IPTV，数字电视   |
|FLV   |Adobe Inc.   |支持   |Sorenson, VP6, H.264   |MP3, ADPCM, Linear PCM, AAC等   |互联网视频网站   |
|MKV   |CoreCodec Inc.   | 支持  |几乎所有格式   | 几乎所有格式  | 互联网视频网站  |
|RMVB   | Real Networks Inc.  | 支持  | RealVideo 8, 9, 10  | AAC, Cook Codec, RealAudio Lossless  | BT下载影视  |

#### 视频编码
|名称|推出机构|推出时间|目前使用领域|
|---|---|---|---|
|HEVC(H.265)   |MPEG/ITU-T   | 2013  |研发中   |
|H.264   |MPEG/ITU-T   |2003   | 各个领域  |
|MPEG4   | MPEG  |  2001 | 不温不火  |
|MPEG2   | MPEG  | 1994  | 数字电视  |
|VP9   |Google   |2013   | 研发中  |
|VP8   |Google  |2008   | 不普及  |
|VC-1   | Microsoft Inc.  | 2006  | 微软平台  |

`H.264(AVC)` 是编码标准，官方编码器是JM，但是速度非常慢，无法商用，使用最多的是开源的 x264，下一代的`H.265(HEVC)`，官方编码器HM，同样很慢，考虑 x265

比较结果是
`HEVC > VP9 > H.264 > VP8 > MPEG4 > H.263 > MPEG2`

##### H.264被替换的原因:
1. 视频清晰度:720P -> 1080P -> 4Kx2K -> 8Kx4K
2. 帧率:30fps -> 60fps -> 120fps -> 240fps
3. 高压缩率

#### 音频编码
|名称|推出机构|推出时间|目前使用领域|
|---|---|---|---|
|AAC   |MPEG   |1997   |各个领域   |
|AC-3   |Dolby Inc.   |1992   |电影   |
|MP3   |MPEG   |1993   |各个领域（旧   |
|WMA   |Microsoft Inc.  |1999   |微软平台   |

因为音频压缩和视频相比影响很小，所以可以选用高效率的音频码率
AAC+ > MP3PRO > AAC> RealAudio > WMA > MP3


### 编码
* 软编码, 升级方便，实现简单，但是CPU负载重，性能低
* 硬编码, 性能高

#### 软编码
使用CPU进行编码

#### 硬编码
使用显卡GPU专用的DSP/FPGA/ASIC芯片等硬件进行编码

#### FFmpeg 组成
AVFormat 封装模块
AVCodec 编解码模块
AVFilter 滤镜模块
AVDevice
AVUtil
swresample 音频转换模块，重采样等
swscale 图像转换模块，缩放、像素格式转换等

#### FFmpeg包组成


## FFmpeg准备
1. 64位ubuntu安装32位兼容库，更新apt-get, 需要sudo
```
sudo apt-get update
sudo apt-get install yasm
sudo apt-get install pkg-config
sudo apt install curl
sudo apt-get install autoconf

```
2. 安装ndk r19c
```
https://dl.google.com/android/repository/android-ndk-r19c-linux-x86_64.zip
unzip android-ndk-r19c-linux-x86_64.zip
```

## FFmpeg 准备
### 1. 下载FFmpeg 4.1.3, 下载地址为github的release
```
https://github.com/FFmpeg/FFmpeg/archive/n4.1.3.tar.gz
tar -zxvf n4.1.3.tar.gz
```

### 2. 编译x264
#### 2.1 安装nasm
```
curl -O -L http://www.nasm.us/pub/nasm/releasebuilds/2.13.01/nasm-2.13.01.tar.bz2
tar xjvf nasm-2.13.01.tar.bz2
cd nasm-2.13.01
./autogen.sh
./configure
make
make install
```

### 2. 编译FFmpeg
#### 2.1 修改 configure(位于ffmpeg根目录)
```
SLIBNAME_WITH_MAJOR='$(SLIBNAME).$(LIBMAJOR)'  
LIB_INSTALL_EXTRA_CMD='$$(RANLIB)"$(LIBDIR)/$(LIBNAME)"'  
SLIB_INSTALL_NAME='$(SLIBNAME_WITH_VERSION)'  
SLIB_INSTALL_LINKS='$(SLIBNAME_WITH_MAJOR)$(SLIBNAME)'  
```

修改为

```
SLIBNAME_WITH_MAJOR='$(SLIBPREF)$(FULLNAME)-$(LIBMAJOR)$(SLIBSUF)'  
LIB_INSTALL_EXTRA_CMD='$$(RANLIB)"$(LIBDIR)/$(LIBNAME)"'  
SLIB_INSTALL_NAME='$(SLIBNAME_WITH_MAJOR)'  
SLIB_INSTALL_LINKS='$(SLIBNAME)'  
```

#### 2.2 新建build.sh

```
#!/bin/bash
NDK=/home/fred/Documents/NDK/android-ndk-r15c
ADDI_LDFLAGS="-fPIE -pie"
ADDI_CFLAGS="-fPIE -pie -march=armv7-a -mfloat-abi=softfp -mfpu=neon"
CPU=armv7-a
ARCH=arm
HOST=arm-linux
SYSROOT=$NDK/toolchains/llvm/prebuilt/linux-x86_64/sysroot
CROSS_PREFIX=$NDK/toolchains/llvm/prebuilt/linux-x86_64/bin/armv7a-linux-androideabi21-
PREFIX=$(pwd)/android/$CPU
x264=$(pwd)/x264/android/$CPU

configure()
{
    ./configure \
    --prefix=$PREFIX \
    --toolchain=clang-usan \
    --disable-encoders \
    --disable-decoders \
    --disable-avdevice \
    --disable-static \
    --disable-doc \
    --disable-ffplay \
    --disable-network \
    --disable-doc \
    --disable-symver \
    --enable-neon \
    --enable-shared \
    --enable-libx264 \
    --enable-gpl \
    --enable-pic \
    --enable-jni \
    --enable-pthreads \
    --enable-mediacodec \
    --enable-encoder=aac \
    --enable-encoder=gif \
    --enable-encoder=libopenjpeg \
    --enable-encoder=libmp3lame \
    --enable-encoder=libwavpack \
    --enable-encoder=libx264 \
    --enable-encoder=mpeg4 \
    --enable-encoder=pcm_s16le \
    --enable-encoder=png \
    --enable-encoder=srt \
    --enable-encoder=subrip \
    --enable-encoder=yuv4 \
    --enable-encoder=text \
    --enable-decoder=aac \
    --enable-decoder=aac_latm \
    --enable-decoder=libopenjpeg \
    --enable-decoder=mp3 \
    --enable-decoder=mpeg4_mediacodec \
    --enable-decoder=pcm_s16le \
    --enable-decoder=flac \
    --enable-decoder=flv \
    --enable-decoder=gif \
    --enable-decoder=png \
    --enable-decoder=srt \
    --enable-decoder=xsub \
    --enable-decoder=yuv4 \
    --enable-decoder=vp8_mediacodec \
    --enable-decoder=h264_mediacodec \
    --enable-decoder=hevc_mediacodec \
    --enable-ffmpeg \
    --enable-bsf=aac_adtstoasc \
    --enable-bsf=h264_mp4toannexb \
    --enable-bsf=hevc_mp4toannexb \
    --enable-bsf=mpeg4_unpack_bframes \
    --enable-cross-compile \
    --cross-prefix=$CROSS_PREFIX \
    --target-os=android \
    --arch=$ARCH \
    --sysroot=$SYSROOT \
    --extra-cflags="-I$x264/include $ADDI_CFLAGS" \
    --extra-ldflags="-L$x264/lib $ADDI_LDFLAGS"
}

build()
{
    make clean

    configure
    make -j4
    make install
}

build
```
#### PS:
* 查看所有编译配置选项：./configure --help
* 查看支持的解码器：./configure --list-decoders
* 查看支持的编码器：./configure --list-encoders
* 查看支持的硬件加速：./configure --list-hwaccels
#### 2.3 编译

```
./configure   #1.build之前需要执行
chmod +x build.sh  #2.赋予脚本执行权限：
sudo ./build.sh  #3.执行脚本开始编译：
```


## 报错汇总
##### 1. nasm/yasm not found or too old. Use --disable-x86asm for a crippled build
* 未安装yasm

##### 权限问题
```
mkdir: cannot create directory ‘/usr/local/share/man/man1’: Permission denied
doc/Makefile:125: recipe for target 'install-man' failed
make: *** [install-man] Error 1

```
* 解决: root 权限 build

##### 2. ffbuild/config.mak: No such file or directory[参考github](https://github.com/dxjia/ffmpeg-compile-shared-library-for-android/issues/9)
* 执行.sh前未执行```./configure```


##### 3. 编译出现问题
* 解决：terminal里的看不懂的话，查看./ffbuild/config.log 里的日记


##### 4. 64位ubuntu安装32位库时报错
安装时的命令行
```
sudo apt-get install ia32-libs
```
报错：
```
Reading package lists... Done
Building dependency tree       
Reading state information... Done
Package ia32-libs is not available, but is referred to by another package.
This may mean that the package is missing, has been obsoleted, or
is only available from another source
However the following packages replace it:
  lib32ncurses5 lib32z1

E: Package 'ia32-libs' has no installation candidate
```
解决:
```
sudo apt-get update
sudo apt-get -y upgrade
apt-get -y install gcc g++ make cmake curl  libcurl3 libcurl3-dev bzip2 pkg-config
```

##### 5. armv7a-linux-androideabi21-clang is unable to create an executable file
可能是build.sh里"--cc=XXX"的路径没有描述，也可能是NDK的路径缺少userName, 比如


##### 6. Makefile:3: config.mak: No such file or directory
./configure
 缺少的是config.mak, 这个文件由./configure 生成
 解决：[Ref:CSDN](https://github.com/dxjia/ffmpeg-compile-shared-library-for-android/issues/9)
  ```
  先执行
  ./configure
  ```

##### 7. Found no assembler
Minimum version is nasm-2.13
If you really want to compile without asm, configure with --disable-asm.
未安装yasm, 或者版本过低
解决:
```
卸载后安装高版本nasm或者直接不要asm
1. 卸载 sudo yum remove nasm && hash -r
卸载后返回
Setting up Remove Process
No Match for argument: nasm
No Packages marked for removal
2. 直接不要asm
./configure --disable-asm
```

##### 8. + autoheader
./autogen.sh: 5: ./autogen.sh: autoheader: not found
解决:安装auto
```
apt-get install autoconf
```

### reference
* [总结]视音频编解码技术零基础学习方法 https://blog.csdn.net/leixiaohua1020/article/details/18893769
* FFmpeg支持的压缩格式和编解码格式 http://notes.maxwi.com/2017/04/05/ffmpeg-codecs-formats/
*
