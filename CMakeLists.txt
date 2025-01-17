#检查cmake版本
cmake_minimum_required(VERSION 3.5)  
#项目名
project(yolov5tensorrt)

#自定义opencv路径
# set(OpenCV_DIR /home/ls/softwares/opencv-4.5.1/build)
# find opencv
find_package( OpenCV REQUIRED )
include_directories( ${OpenCV_INCLUDE_DIRS})

message(STATUS "Find OpenCV include at ${OpenCV_INCLUDE_DIRS}")
message(STATUS "Find OpenCV libraries: ${OpenCV_LIBRARIES}")

if(NOT OpenCV_FOUND)
    message(ERROR "OpenCV not found!")
endif(NOT OpenCV_FOUND)
#cuda
find_package(CUDA REQUIRED)
# enable_language(CUDA)
message(STATUS "CUDA_LIBRARIES => ${CUDA_LIBRARIES}")
message(STATUS "CUDA_INCLUDE_DIRS => ${CUDA_INCLUDE_DIRS}")
include_directories("/usr/local/cuda/include")
link_directories("/usr/local/cuda/lib64" "/usr/lib" /usr/lib/x86_64-linux-gnu )
#tensorrt
set(TensorRT_DIR /home/ls/softwares/TensorRT-7.2.3.4)
if(DEFINED TensorRT_DIR)
  include_directories("${TensorRT_DIR}/include")
  link_directories("${TensorRT_DIR}/lib")
endif(DEFINED TensorRT_DIR)
include_directories(${CUDA_INCLUDE_DIRS})
message(STATUS "TensorRT Header => ${tensorrt_dir}/include")
message(STATUS "TensorRT Lib => ${tensorrt_dir}/lib")

set(TRT_LIB libnvinfer.so libnvonnxparser.so cudart.so)

add_subdirectory(${CMAKE_SOURCE_DIR}/module)
include_directories(${CMAKE_SOURCE_DIR}/module/include)
# 链接库目录
link_directories(${CMAKE_SOURCE_DIR}/lib)
# 链接依赖库
link_libraries(module)

ADD_EXECUTABLE(${PROJECT_NAME} main.cpp)
target_link_libraries(${PROJECT_NAME} ${TRT_LIB} ${OpenCV_LIBS} ${CUDA_LIBRARIES})








