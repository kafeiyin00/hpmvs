cmake_minimum_required (VERSION 2.8.9) 
project (HPMVS)

ADD_DEFINITIONS(
    -std=c++11
    -O3
)

list(APPEND CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake")

find_package(stlplus REQUIRED)
#find_package(Loccom REQUIRED)
find_package(Eigen3 REQUIRED)
find_package(JPEG REQUIRED)
find_package(Gflags REQUIRED)
find_package(Glog REQUIRED)

ADD_SUBDIRECTORY(thirdLibs/nlopt-2.4.2)

set(MY_SRC 
	src/hpmvs/NVMReader.cpp
	src/hpmvs/doctree.cpp
	src/hpmvs/CellProcessor.cpp
	src/hpmvs/PatchOptimizer.cpp
	src/hpmvs/Patch3d.cpp
	src/hpmvs/Scene.cpp
	src/hpmvs/Image.cpp
	src/hpmvs/Camera.cpp
	)

find_package(OpenMP)
if (OPENMP_FOUND)
	    set (CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")
	        set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
endif()


include_directories( 
	"${PROJECT_SOURCE_DIR}/include"
        ${STLPLUS_INCLUDE_DIRS}
	${EIGEN3_INCLUDE_DIR}
	${JPEG_INCLUDE_DIR}
	${GFLAGS_INCLUDE_DIRS} 
	${GLOG_INCLUDE_DIRS}
	${CMAKE_SOURCE_DIR}/thirdLibs/nlopt-2.4.2/api
	${CMAKE_SOURCE_DIR}/thirdLibs/cimg
	)


add_executable (hpmvs src/main.cpp ${MY_SRC})
target_link_libraries(hpmvs ${STLPLUS_LIBRARIES} ${JPEG_LIBRARIES} ${GFLAGS_LIBRARIES} ${GLOG_LIBRARIES} nlopt)

install(TARGETS hpmvs RUNTIME DESTINATION bin)
