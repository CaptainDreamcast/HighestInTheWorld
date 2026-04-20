cmake_minimum_required(VERSION 3.25) 
project(LD59 CXX)               
set(CMAKE_CXX_STANDARD 17)            
set(CMAKE_MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>")

include("${CMAKE_SOURCE_DIR}/../../addons/prism/cmake/prism.cmake" REQUIRED)

# Add source/header files to project
file(GLOB_RECURSE SOURCES ../*.cpp SOURCES ../*.h SOURCES ../*.rc)
list(FILTER SOURCES EXCLUDE REGEX ".*web.*")
list(FILTER SOURCES EXCLUDE REGEX ".*/build/.*")

add_link_options(/NODEFAULTLIB:libcmt.lib)
add_link_options(/IGNORE:4099,4286,4098)
add_link_options(/OPT:REF)
add_link_options(/OPT:ICF)

# Define exe
add_executable(LD59 ${SOURCES})

# Link with libraries
target_link_libraries(LD59 prism)
target_link_libraries(LD59 SDL2)
target_link_libraries(LD59 SDL2main)
target_link_libraries(LD59 SDL2_image)
target_link_libraries(LD59 fmod)
target_link_libraries(LD59 SDL2_ttf)
target_link_libraries(LD59 enet)
target_link_libraries(LD59 freetype)
target_link_libraries(LD59 glew)
target_link_libraries(LD59 curl)
target_link_libraries(LD59 libjpeg)
target_link_libraries(LD59 libpng)
target_link_libraries(LD59 libwebp)
target_link_libraries(LD59 zstd)
target_link_libraries(LD59 zlib)
target_link_libraries(LD59 crypt32)
target_link_libraries(LD59 wldap32)
target_link_libraries(LD59 ws2_32)
target_link_libraries(LD59 winmm)
target_link_libraries(LD59 OpenGL32)
target_link_libraries(LD59 version)

# Define preprocessor definitions
target_compile_definitions(LD59 PUBLIC UNICODE)
target_compile_definitions(LD59 PUBLIC _UNICODE)

set_property(TARGET LD59 PROPERTY INTERPROCEDURAL_OPTIMIZATION TRUE)
target_compile_options(LD59 PRIVATE /Gy)

set_target_properties(LD59 PROPERTIES RUNTIME_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/../assets)
set_target_properties(LD59 PROPERTIES RUNTIME_OUTPUT_DIRECTORY_DEBUG ${CMAKE_SOURCE_DIR}/../assets)

# Copy over DLLs
file(GLOB DLLS "${CMAKE_SOURCE_DIR}/../../addons/prism/windows/vs17/DLL/*.dll")
foreach(DLL ${DLLS})
  add_custom_command(TARGET LD59 POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_if_different
    ${DLL}
    $<TARGET_FILE_DIR:LD59>)
endforeach()

# Optimize for release build
if(CMAKE_BUILD_TYPE STREQUAL "Release")
  target_compile_options(LD59 PRIVATE /O2)
endif()
