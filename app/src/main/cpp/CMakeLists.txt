## For more information about using CMake with Android Studio, read the
## documentation: https://d.android.com/studio/projects/add-native-code.html
#
## Sets the minimum version of CMake required to build the native library.
#
#cmake_minimum_required(VERSION 3.18.1)
#
## Declares and names the project.
#
#project("studyandroidffmpeg")
#
## Creates and names a library, sets it as either STATIC
## or SHARED, and provides the relative paths to its source code.
## You can define multiple libraries, and CMake builds them for you.
## Gradle automatically packages shared libraries with your APK.
#
## 定义so库和头文件所在目录，方便后面使用
#set(FFmpeg_lib_dir ${CMAKE_SOURCE_DIR}/../jniLibs/${CMAKE_ANDROID_ARCH_ABI})
#
## 添加头文件目录
#include_directories(
#        include
#)
#
#add_library( # Sets the name of the library.
#        studyandroidffmpeg
#
#        # Sets the library as a shared library.
#        SHARED
#
#        # Provides a relative path to your source file(s).
#        native-lib.cpp)
#
## 添加FFmpeg相关的so库
## 添加库——外部引入的库
## 库名称：avcodec（不需要包含前缀lib）
## 库类型：SHARED，表示动态库，后缀为.so（如果是STATIC，则表示静态库，后缀为.a）
## IMPORTED表明是外部引入的库
#
#add_library(
#        avcodec
#        SHARED
#        IMPORTED
#)
## 设置目标属性
## 设置avcodec目标库的IMPORTED_LOCATION属性，用于说明引入库的位置
## 还可以设置其他属性，格式：PROPERTIES key value
#set_target_properties(
#        avcodec
#        PROPERTIES IMPORTED_LOCATION
#        ${FFmpeg_lib_dir}/libavcodec.so
#)
#
#add_library(
#        avfilter
#        SHARED
#        IMPORTED
#)
#set_target_properties(
#        avfilter
#        PROPERTIES IMPORTED_LOCATION
#        ${FFmpeg_lib_dir}/libavfilter.so
#)
#
#add_library(
#        avformat
#        SHARED
#        IMPORTED
#)
#set_target_properties(
#        avformat
#        PROPERTIES IMPORTED_LOCATION
#        ${FFmpeg_lib_dir}/libavformat.so
#)
#
#add_library(
#        avutil
#        SHARED
#        IMPORTED
#)
#set_target_properties(
#        avutil
#        PROPERTIES IMPORTED_LOCATION
#        ${FFmpeg_lib_dir}/libavutil.so
#)
#
#add_library(
#        swresample
#        SHARED
#        IMPORTED
#)
#set_target_properties(
#        swresample
#        PROPERTIES IMPORTED_LOCATION
#        ${FFmpeg_lib_dir}/libswresample.so
#)
#
#add_library(
#        swscale
#        SHARED
#        IMPORTED
#)
#set_target_properties(
#        swscale
#        PROPERTIES IMPORTED_LOCATION
#        ${FFmpeg_lib_dir}/libswscale.so
#)
#
## Searches for a specified prebuilt library and stores the path as a
## variable. Because CMake includes system libraries in the search path by
## default, you only need to specify the name of the public NDK library
## you want to add. CMake verifies that the library exists before
## completing its build.
#
##NDK自带log库
#find_library( # Sets the name of the path variable.
#        log-lib
#
#        # Specifies the name of the NDK library that
#        # you want CMake to locate.
#        log)
#
#
## Specifies libraries CMake should link to your target library. You
## can link multiple libraries, such as libraries you define in this
## build script, prebuilt third-party libraries, or system libraries.
#
## 引入头文件
##include_directories("${CMAKE_SOURCE_DIR}/include")
#
#target_link_libraries( # Specifies the target library.
#        studyandroidffmpeg
#        avcodec
#        avfilter
#        avformat
#        avutil
#        swresample
#        swscale
#        -landroid
#        # Links the target library to the log library
#        # included in the NDK.
#        ${log-lib})


# Sets the minimum version of CMake required to build the native library.
cmake_minimum_required(VERSION 3.4.1)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=gnu++11")

set(jnilibs ${CMAKE_SOURCE_DIR}/../jniLibs)
set(libname learn-ffmpeg)

include_directories(
        include
        ${CMAKE_SOURCE_DIR}/util
)

link_directories(
        ${jnilibs}/${ANDROID_ABI})

file(GLOB src-files
        ${CMAKE_SOURCE_DIR}/*.cpp)

add_library( # Sets the name of the library.
        ${libname}

        # Sets the library as a shared library.
        SHARED

        # Provides a relative path to your source file(s).
        ${src-files}
        )

set(third-party-libs
        avformat
        avcodec
        avfilter
        swresample
        swscale
        avutil
        )

set(native-libs
        android
        EGL
        GLESv3
        OpenSLES
        log
        m
        z
        )

target_link_libraries( # Specifies the target library.
        ${libname}

        # Links the target library to the log library
        # included in the NDK.
        ${log-lib}
        ${third-party-libs}
        ${native-libs}
        )
