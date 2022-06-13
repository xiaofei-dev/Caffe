cmake_minimum_required(VERSION 2.8)
project(caffe_jni LANGUAGES C CXX)
#编译优化选项，重要！！！一定要设置好
set(CMAKE_CXX_FLAGS_DEBUG  "${CMAKE_CXX_FLAGS_DEBUG} -Ofast")
set(CMAKE_CXX_FLAGS_RELEASE  "${CMAKE_CXX_FLAGS_RELEASE} -Ofast")

set(CMAKE_C_FLAGS_DEBUG  "${CMAKE_C_FLAGS_DEBUG} -Ofast")
set(CMAKE_C_FLAGS_RELEASE  "${CMAKE_C_FLAGS_RELEASE} -Ofast")
#头文件
set(ANDROID_DEPRECATED_HEADERS on)
#开始 OpenCV
set(CMAKE_VERBOSE_MAKEFILE on)
#set(libs "${CMAKE_SOURCE_DIR}/src/main/jniLibs")
set(libs "${CMAKE_SOURCE_DIR}/jniLibs")
include_directories(${CMAKE_SOURCE_DIR}/include)

#include_directories(${CMAKE_SOURCE_DIR}/src/main/cpp/include/opencv2)

add_library(libopencv_calib3d STATIC IMPORTED )
set_target_properties(libopencv_calib3d PROPERTIES
        IMPORTED_LOCATION "${libs}/${ANDROID_ABI}/libopencv_calib3d.a")

add_library(libopencv_core STATIC IMPORTED )
set_target_properties(libopencv_core PROPERTIES
        IMPORTED_LOCATION "${libs}/${ANDROID_ABI}/libopencv_core.a")

add_library(libopencv_java3 SHARED IMPORTED )
set_target_properties(libopencv_java3 PROPERTIES
        IMPORTED_LOCATION "${libs}/${ANDROID_ABI}/libopencv_java3.so")

add_library(libopencv_features2d STATIC IMPORTED )
set_target_properties(libopencv_features2d PROPERTIES
        IMPORTED_LOCATION "${libs}/${ANDROID_ABI}/libopencv_features2d.a")

add_library(libopencv_flann STATIC IMPORTED )
set_target_properties(libopencv_flann PROPERTIES
        IMPORTED_LOCATION "${libs}/${ANDROID_ABI}/libopencv_flann.a")

add_library(libopencv_highgui STATIC IMPORTED )
set_target_properties(libopencv_highgui PROPERTIES
        IMPORTED_LOCATION "${libs}/${ANDROID_ABI}/libopencv_highgui.a")

add_library(libopencv_imgcodecs STATIC IMPORTED )
set_target_properties(libopencv_imgcodecs PROPERTIES
        IMPORTED_LOCATION "${libs}/${ANDROID_ABI}/libopencv_imgcodecs.a")

add_library(libopencv_imgproc STATIC IMPORTED )
set_target_properties(libopencv_imgproc PROPERTIES
        IMPORTED_LOCATION "${libs}/${ANDROID_ABI}/libopencv_imgproc.a")

add_library(libopencv_ml STATIC IMPORTED )
set_target_properties(libopencv_ml PROPERTIES
        IMPORTED_LOCATION "${libs}/${ANDROID_ABI}/libopencv_ml.a")

add_library(libopencv_objdetect STATIC IMPORTED )
set_target_properties(libopencv_objdetect PROPERTIES
        IMPORTED_LOCATION "${libs}/${ANDROID_ABI}/libopencv_objdetect.a")

add_library(libopencv_photo STATIC IMPORTED )
set_target_properties(libopencv_photo PROPERTIES
        IMPORTED_LOCATION "${libs}/${ANDROID_ABI}/libopencv_photo.a")

add_library(libopencv_shape STATIC IMPORTED )
set_target_properties(libopencv_shape PROPERTIES
        IMPORTED_LOCATION "${libs}/${ANDROID_ABI}/libopencv_shape.a")

add_library(libopencv_stitching STATIC IMPORTED )
set_target_properties(libopencv_stitching PROPERTIES
        IMPORTED_LOCATION "${libs}/${ANDROID_ABI}/libopencv_stitching.a")

add_library(libopencv_superres STATIC IMPORTED )
set_target_properties(libopencv_superres PROPERTIES
        IMPORTED_LOCATION "${libs}/${ANDROID_ABI}/libopencv_superres.a")

add_library(libopencv_video STATIC IMPORTED )
set_target_properties(libopencv_video PROPERTIES
        IMPORTED_LOCATION "${libs}/${ANDROID_ABI}/libopencv_video.a")

add_library(libopencv_videoio STATIC IMPORTED )
set_target_properties(libopencv_videoio PROPERTIES
        IMPORTED_LOCATION "${libs}/${ANDROID_ABI}/libopencv_videoio.a")

add_library(libopencv_videostab STATIC IMPORTED )
set_target_properties(libopencv_videostab PROPERTIES
        IMPORTED_LOCATION "${libs}/${ANDROID_ABI}/libopencv_videostab.a")

add_library(libopencv_ts STATIC IMPORTED )
set_target_properties(libopencv_ts PROPERTIES
        IMPORTED_LOCATION "${libs}/${ANDROID_ABI}/libopencv_ts.a")

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=gnu++11 -fexceptions -frtti")

#include_directories(D:/Projects/Android/CLMAndroid/OpenCV-android-sdk/sdk/native/jni/include )
#set(OpenCV_DIR D:/Projects/Android/CLMAndroid/OpenCV-android-sdk/sdk/native/jni)
#find_package(OpenCV REQUIRED)
#target_link_libraries(${OpenCV_LIBS})
#-------------------------------------------------OpenCV 结束

add_definitions(-DCPU_ONLY)

add_compile_options(-std=c++11)

include_directories(${CMAKE_SOURCE_DIR}/third_party/caffe/include
        ${CMAKE_SOURCE_DIR}/third_party/boost/include
        ${CMAKE_SOURCE_DIR}/third_party/gflags/include
        ${CMAKE_SOURCE_DIR}/third_party/glog/include
        ${CMAKE_SOURCE_DIR}/third_party/protobuf/include
        ${CMAKE_SOURCE_DIR}/third_party/openblas/include)

#[[add_library(libboost_system STATIC IMPORTED)
set_target_properties(libboost_system
        PROPERTIES
        IMPORTED_LOCATION
        "${libs}/armeabi-v7a/libboost_system.a")]]

add_library(libcaffe SHARED IMPORTED)
set_target_properties(libcaffe
        PROPERTIES
        IMPORTED_LOCATION
        "${libs}/${ANDROID_ABI}/libcaffe.so")

add_library(caffe_jni
        SHARED

        caffe_jni.cpp
        caffe_mobile.cpp
        caffe_mobile.hpp)

#add_executable(caffe_jni caffe_jni.cpp caffe_mobile.cpp)
#target_link_libraries(caffe_jni ${Caffe_LINK})

#caffe_default_properties(caffe_jni)

# set back RUNTIME_OUTPUT_DIRECTORY
set_target_properties(caffe_jni PROPERTIES
        RUNTIME_OUTPUT_DIRECTORY "${PROJECT_BINARY_DIR}/android")

#caffe_set_solution_folder(caffe_jni android)

# install
#install(TARGETS caffe_jni DESTINATION lib)

find_library( # Sets the name of the path variable.
        log-lib

        # Specifies the name of the NDK library that
        # you want CMake to locate.
        log )

target_link_libraries(caffe_jni
        libopencv_java3
        libopencv_calib3d
        libopencv_core
        libopencv_features2d
        libopencv_flann
        libopencv_highgui
        libopencv_imgcodecs
        libopencv_imgproc
        libopencv_ml
        libopencv_objdetect
        libopencv_photo
        libopencv_shape
        libopencv_stitching
        libopencv_superres
        libopencv_video
        libopencv_videoio
        libopencv_videostab
        jnigraphics
        ${log-lib}
        #libboost_system
        libcaffe
        )
