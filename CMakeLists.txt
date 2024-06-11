cmake_minimum_required (VERSION 2.6...3.22.1)

project(argus_encode)

set(CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake" "${CMAKE_MODULE_PATH}")

find_package(Argus REQUIRED)
find_package(OpenGLES REQUIRED)
find_package(EGL REQUIRED)
find_package(OpenCV REQUIRED)

include_directories(include)
include_directories(${OpenCV_INCLUDE_DIRS})

add_subdirectory(utils)

find_library(NVOSD_LIBRARY NAMES nvosd 
             HINTS /usr/lib/aarch64-linux-gnu/nvidia)

find_library(NVJPEG_LIBRARY NAMES nvjpeg 
             HINTS /usr/lib/aarch64-linux-gnu/nvidia)

find_library(NVBUFSURF_LIBRARY NAMES nvbufsurftransform 
             HINTS /usr/lib/aarch64-linux-gnu/nvidia)

add_executable(${PROJECT_NAME} 
    src/main.cpp
    src/NvEglRenderer.cpp
    src/NvLogging.cpp
    src/NvJpegEncoder.cpp
    src/NvElement.cpp
    src/NvElementProfiler.cpp
    src/NvBufSurface.cpp
)

link_directories(
    /${CUDA_PATH}/lib64
    /usr/lib/aarch64-linux-gnu
    /usr/lib/aarch64-linux-gnu/nvidia
    /usr/lib/aarch64-linux-gnu/tegra
)

target_link_libraries(${PROJECT_NAME}
    ${ARGUS_LIBRARIES}
    ${OpenCV_LIBS}
    argus_utils
)

target_link_libraries(${PROJECT_NAME}
    ${NVOSD_LIBRARY}
    ${NVJPEG_LIBRARY}
    ${NVBUFSURF_LIBRARY}
)

# link_directories(/usr/lib/aarch64-linux-gnu/tegra)
