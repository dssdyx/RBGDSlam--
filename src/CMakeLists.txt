
message("cmake_module_path: ${CMAKE_MODULE_PATH}")
FIND_PACKAGE( PCL REQUIRED COMPONENTS common io visualization filters )
list(REMOVE_ITEM PCL_LIBRARIES "vtkproj4")



include_directories("/usr/include/eigen3")


FIND_PACKAGE( OpenCV REQUIRED )
INCLUDE_DIRECTORIES( ${OpenCV_INCLUDE_DIRS} )

ADD_DEFINITIONS( ${PCL_DEFINITIONS} )
INCLUDE_DIRECTORIES( ${PCL_INCLUDE_DIRS}  )
LINK_LIBRARIES( ${PCL_LIBRARY_DIRS} )


# 添加g2o的依赖
# 因为g2o不是常用库，要添加它的findg2o.cmake文件
SET( G2O_ROOT /usr/local/include/g2o )
FIND_PACKAGE( G2O REQUIRED )
# CSparse
FIND_PACKAGE( CSparse REQUIRED )
INCLUDE_DIRECTORIES( ${G2O_INCLUDE_DIR} ${CSPARSE_INCLUDE_DIR} )



ADD_EXECUTABLE( pcl_pic generatePointCloud.cpp )
TARGET_LINK_LIBRARIES( pcl_pic ${OpenCV_LIBS}
    ${PCL_LIBRARIES} )

ADD_LIBRARY( slambase slamBase.cpp )
TARGET_LINK_LIBRARIES( slambase
    ${OpenCV_LIBS}
    ${PCL_LIBRARIES} )

add_executable(main detectFeatures.cpp)
TARGET_LINK_LIBRARIES( main
    slambase
    ${OpenCV_LIBS}
    ${PCL_LIBRARIES} )

add_executable(main2 joinPointCloud.cpp)
TARGET_LINK_LIBRARIES( main2
    slambase
    ${OpenCV_LIBS}
    ${PCL_LIBRARIES} )

add_executable(main3 visualOdometry.cpp)
TARGET_LINK_LIBRARIES( main3
    slambase
    ${OpenCV_LIBS}
    ${PCL_LIBRARIES} )

add_executable(main4 slamEnd.cpp)
TARGET_LINK_LIBRARIES( main4
    slambase
    ${OpenCV_LIBS}
    ${PCL_LIBRARIES}
    g2o_core g2o_types_slam3d g2o_solver_csparse g2o_stuff g2o_csparse_extension
    ${CSPARSE_LIBRARY})
