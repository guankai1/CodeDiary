### #ifdef, #ifndef, #endif
c语言中, 对同一个变量/函数多次声明不会报错, 即使多个c文件包含同一个h文件也可以
c++中, #ifdef 的作用域是单个文件, 所以如果h文件里定义了全局变量, 即使采用#ifdef, 多个c文件包含同一个h文件会出现全局变量重定义的错误
所以用 #ifndef  先判断一下
记得都需要用 #endif 收尾


### 路线
* 基本数据
* 指针类型
* 动态内存分配
*  结构体指针
*  联合体&枚举
* JNI 流程
* JNI 数据类型
* JNI 数组处理
* JNI 缓存策略
* NDK 文件
* NDK增量更新
* C++ 命名空间
* C++ 构造函数
* C++ 初始化 静态成员 this 指针
* C++ 多态， 模板函数
* fmod 编译
* fmod QQ变声特效
* ffmpeg 编译
*  视频解码
*  像素格式转换与 native 原生绘制
*  音频解码
*  AudioTrack 音频播放
* shell 命令
* linux 文件比较 循环机制
* linux 脚本传参  标准输出重定向函数
* Android.mk
* pthread_POSIX 线程原语
* linux JNI 多线程1
* linux JNI 多线程2
*
