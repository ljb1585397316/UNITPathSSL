# Copyright (c) Facebook, Inc. and its affiliates.
# See https://pytorch.org/tutorials/advanced/cpp_frontend.html
cmake_minimum_required(VERSION 3.12 FATAL_ERROR)
project(caffe2_mask_rcnn)

find_package(Torch REQUIRED)
find_package(gflags REQUIRED)  # needed by caffe2
find_package(OpenCV REQUIRED)
find_package(TorchVision REQUIRED)   # needed by export-method=tracing/scripting

add_executable(caffe2_mask_rcnn caffe2_mask_rcnn.cpp)
target_link_libraries(
  caffe2_mask_rcnn
  "${TORCH_LIBRARIES}" gflags glog protobuf ${OpenCV_LIBS})
set_property(TARGET caffe2_mask_rcnn PROPERTY CXX_STANDARD 14)


add_executable(torchscript_mask_rcnn torchscript_mask_rcnn.cpp)
target_link_libraries(
  torchscript_mask_rcnn
  -Wl,--no-as-needed TorchVision::TorchVision -Wl,--as-needed
  "${TORCH_LIBRARIES}" ${OpenCV_LIBS})
set_property(TARGET torchscript_mask_rcnn PROPERTY CXX_STANDARD 14)
