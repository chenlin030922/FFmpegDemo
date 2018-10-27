# For more information about using CMake with Android Studio, read the
# documentation: https://d.android.com/studio/projects/add-native-code.html

# Sets the minimum version of CMake required to build the native library.

cmake_minimum_required(VERSION 3.4.1)

# Creates and names a library, sets it as either STATIC
# or SHARED, and provides the relative paths to its source code.
# You can define multiple libraries, and CMake builds them for you.
# Gradle automatically packages shared libraries with your APK.

add_library( # Sets the name of the library.
        native-lib

        # Sets the library as a shared library.
        SHARED

        # Provides a relative path to your source file(s).
        src/main/cpp/native-lib.cpp)

# Searches for a specified prebuilt library and stores the path as a
# variable. Because CMake includes system libraries in the search path by
# default, you only need to specify the name of the public NDK library
# you want to add. CMake verifies that the library exists before
# completing its build.

find_library( # Sets the name of the path variable.
        log-lib

        # Specifies the name of the NDK library that
        # you want CMake to locate.
        log)

# Specifies libraries CMake should link to your target library. You
# can link multiple libraries, such as libraries you define in this
# build script, prebuilt third-party libraries, or system libraries.
set(distribution_DIR ${CMAKE_SOURCE_DIR}/src/main/jni/armeabi)
add_library( avutil
             SHARED
             IMPORTED )
set_target_properties( avutil
                       PROPERTIES IMPORTED_LOCATION
                       ${distribution_DIR}/libavutil.so)
add_library( swresample
             SHARED
             IMPORTED )
set_target_properties( swresample
                       PROPERTIES IMPORTED_LOCATION
                       ${distribution_DIR}/libswresample.so)
add_library( avcodec
             SHARED
             IMPORTED )
set_target_properties( avcodec
                       PROPERTIES IMPORTED_LOCATION
                       ${distribution_DIR}/libavcodec.so)
add_library( avfilter
             SHARED
             IMPORTED )
set_target_properties( avfilter
                       PROPERTIES IMPORTED_LOCATION
                       ${distribution_DIR}/libavfilter.so)
add_library( swscale
             SHARED
             IMPORTED )
set_target_properties( swscale
                       PROPERTIES IMPORTED_LOCATION
                       ${distribution_DIR}/libswscale.so)
add_library( avdevice
             SHARED
             IMPORTED )
set_target_properties( avdevice
                       PROPERTIES IMPORTED_LOCATION
                       ${distribution_DIR}/libavdevice.so)
add_library( avformat
             SHARED
             IMPORTED )
set_target_properties( avformat
                       PROPERTIES IMPORTED_LOCATION
                       ${distribution_DIR}/libavformat.so)
add_library( postproc
             SHARED
             IMPORTED )
set_target_properties( postproc
                       PROPERTIES IMPORTED_LOCATION
                       ${distribution_DIR}/libpostproc.so)
set(CMAKE_VERBOSE_MAKEFILE on)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=gnu++11")
include_directories(src/main/cpp)
include_directories(src/main/jni/include)

target_link_libraries( # Specifies the target library.
        native-lib
        avutil       #工具库(大部分需要)
        swresample    #音频采样数据格式转换
        avcodec      #编解码(重要)
        avfilter      #滤镜特效处理
        swscale       #视频像素数据格式转换
        avdevice     #各种设备的输入输出
        avformat     #封装格式处理
        postproc     #后加工


        # Links the target library to the log library
        # included in the NDK.
        ${log-lib})