cmake_minimum_required(VERSION 3.0)
project(ICEDemo)
# Add executable called "helloDemo" that is built from the source files
# "demo.cxx" and "demo_b.cxx". The extensions are automatically found.


# Link the executable to the Hello library. Since the Hello library has
# public include directories we will use those link directories when building
# helloDemo

find_package(PkgConfig REQUIRED)
if (PKG_CONFIG_FOUND)
  pkg_search_module(ICE REQUIRED "nice")
  #add_library(nice SHARED IMPORTED)
  if (ICE_FOUND)
    message(STATUS "ICE library found at ->> ${ICE_INCLUDE_DIRS}" )
    link_directories(${ICE_LIBRARY_DIRS})
    #### perform the operations on the three examples #### 

    add_executable (SDPDemo sdp-example.c )
    target_include_directories(SDPDemo PUBLIC ${ICE_INCLUDE_DIRS}) 
    target_link_libraries(SDPDemo ${ICE_LIBRARIES})

    add_executable (Simple simple-example.c )
    target_include_directories(Simple PUBLIC ${ICE_INCLUDE_DIRS}) 
    target_link_libraries(Simple ${ICE_LIBRARIES})


    add_executable (ThreadedDemo sdp-example.c )  
    target_include_directories(ThreadedDemo PUBLIC ${ICE_INCLUDE_DIRS}) 
    target_link_libraries(ThreadedDemo ${ICE_LIBRARIES})

    #####add stun client targets####
    add_executable (Stunbdc stunbdc.c )
    target_include_directories(Stunbdc PUBLIC ${ICE_INCLUDE_DIRS})
    target_link_libraries(Stunbdc ${ICE_LIBRARIES})

    # add the binary tree to the search path for include files
    # so that we will find TutorialConfig.h
    include_directories("${PROJECT_BINARY_DIR}")
    # The version number shown by the parser software that matters.
    set (PARSE_VERSION_MAJOR "0")
    set (PARSE_VERSION_MINOR "001")
    set (PARSE_VERSION_PATCH "0")
    # configure a header file to pass some of the CMake settings
    # to the source code
    configure_file (
      "${PROJECT_SOURCE_DIR}/VersionConfig.h.in"
      "${PROJECT_BINARY_DIR}/VersionConfig.h"
    )
    add_executable(Stund stund.c )
    target_include_directories(Stund PUBLIC ${ICE_INCLUDE_DIRS})
    target_link_libraries(Stund ${ICE_LIBRARIES})
  endif()
endif()

#the versioning of the release tools that the system is to provide 
set(CPACK_PACKAGE_VERSION_MAJOR ${PARSE_VERSION_MAJOR})
set(CPACK_PACKAGE_VERSION_MINOR ${PARSE_VERSION_MINOR})
set(CPACK_PACKAGE_VERSION_PATCH ${PARSE_VERSION_PATCH})
set(LIB_VERSION_STRING ${CPACK_PACKAGE_VERSION_MAJOR}.${CPACK_PACKAGE_VERSION_MINOR}.${CPACK_PACKAGE_VERSION_PATCH})
set_target_properties(${PROGRAM_NAME} PROPERTIES VERSION ${LIB_VERSION_STRING})
set (CPACK_RESOURCE_FILE_LICENSE  
     "${CMAKE_CURRENT_SOURCE_DIR}/LICENSE")
set(CPACK_PACKAGE_CONTACT "Support on toy-language <adysenlab@gmail.com>")


set(CPACK_GENERATOR "DEB")
include (CPack)