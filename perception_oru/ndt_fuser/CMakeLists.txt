cmake_minimum_required(VERSION 2.4.6)
include($ENV{ROS_ROOT}/core/rosbuild/rosbuild.cmake)

# Set the build type.  Options are:
#  Coverage       : w/ debug symbols, w/o optimization, w/ code-coverage
#  Debug          : w/ debug symbols, w/o optimization
#  Release        : w/o debug symbols, w/ optimization
#  RelWithDebInfo : w/ debug symbols, w/ optimization
#  MinSizeRel     : w/o debug symbols, w/ optimization, stripped binaries
#set(ROS_BUILD_TYPE RelWithDebInfo)

rosbuild_init()
find_package(LibXml2 REQUIRED)
include_directories(${LIBXML2_INCLUDE_DIR})
ADD_DEFINITIONS(-DLINUX_OS)
find_package(OpenGL REQUIRED)
find_package(GLU REQUIRED)
find_package(GLUT REQUIRED)
find_package(JPEG REQUIRED)
include_directories(${JPEG_INCLUDE_DIR})
include_directories(${GLUT_INCLUDE_DIR})
include_directories(${OPENGL_INCLUDE_DIR})

FIND_PACKAGE(MRPT REQUIRED graphslam;gui)

#set the default path for built executables to the "bin" directory
set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
#set the default path for built libraries to the "lib" directory
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

#uncomment if you have defined messages
#rosbuild_genmsg()
#uncomment if you have defined services
#rosbuild_gensrv()
rosbuild_add_boost_directories()

#common commands for building c++ executables and libraries
#rosbuild_add_library(${PROJECT_NAME} src/example.cpp)
#target_link_libraries(${PROJECT_NAME} another_library)
#rosbuild_add_boost_directories()
#rosbuild_link_boost(${PROJECT_NAME} thread)
rosbuild_add_executable(ndt_fuser src/ndt_fuser_node.cpp)
target_link_libraries(ndt_fuser ${LIBXML2_LIBRARIES} ${GLUT_LIBRARIES} ${JPEG_LIBRARIES} ${OPENGL_LIBRARIES} ${MRPT_LIBS})
rosbuild_link_boost(ndt_fuser signals thread)
rosbuild_add_openmp_flags(ndt_fuser)

rosbuild_add_executable(test_ndt_fuser src/ndt_fuser_from_depth.cpp)
target_link_libraries(test_ndt_fuser ${LIBXML2_LIBRARIES} ${GLUT_LIBRARIES} ${JPEG_LIBRARIES} ${OPENGL_LIBRARIES} ${MRPT_LIBS})
rosbuild_link_boost(test_ndt_fuser signals program_options thread)
rosbuild_add_openmp_flags(test_ndt_fuser)


#target_link_libraries(example ${PROJECT_NAME})
