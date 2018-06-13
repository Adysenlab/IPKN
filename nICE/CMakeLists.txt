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
  endif()
endif()
