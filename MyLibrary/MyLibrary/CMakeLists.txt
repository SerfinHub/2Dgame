#Variable This - Project name
set(This MyLibrary)

project(${This} CXX)

set(SOURCES
    src/GodotLibrary.cpp
    include/Controller.h
    src/Controller.cpp
    )

if(EMSCRIPTEN)
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -s ASSERTIONS=1 -s SIDE_MODULE=1")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -s ASSERTIONS=1 -s SIDE_MODULE=1")
    set(CMAKE_EXECUTABLE_SUFFIX ".wasm")
    
    add_executable(${This} ${SOURCES})
else()
    add_library(${This} SHARED ${SOURCES})
endif()

target_link_libraries(${This} PUBLIC godot-cpp)

target_include_directories(${This}
                           PUBLIC
                           $<INSTALL_INTERFACE:include>
                           $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
                           PRIVATE
                           ${CMAKE_CURRENT_SOURCE_DIR}/src
                           )