cmake_minimum_required (VERSION 3.0)
project (tutorial06_keyboard_and_mouse)

set (CMAKE_CXX_FLAGS "-std=c++11")

set (RESOURCES
    TextureFragmentShader.fragmentshader
    TransformVertexShader.vertexshader
    uvtemplate.DDS
    # appicon.icns
)

aux_source_directory (. SRC_LIST)
add_executable (${PROJECT_NAME} ${SRC_LIST} ${RESOURCES})

add_subdirectory (../common ${CMAKE_BINARY_DIR}/common)
target_link_libraries (${PROJECT_NAME} common)
include_directories (..)

add_definitions (-DGLM_FORCE_RADIANS)

include (FindPkgConfig)

pkg_check_modules (GLFW REQUIRED glfw3)
include_directories (${GLFW_INCLUDE_DIRS})
target_link_libraries (${PROJECT_NAME} ${GLFW_LIBRARIES})

if (APPLE)
    set_source_files_properties (${RESOURCES} PROPERTIES MACOSX_PACKAGE_LOCATION Resources)
    set_target_properties (${PROJECT_NAME} PROPERTIES MACOSX_BUNDLE TRUE)
    # set (MACOSX_BUNDLE_ICON_FILE appicon)
endif ()

if (UNIX)
    pkg_check_modules (GLEW REQUIRED glew)
    include_directories (${GLEW_INCLUDE_DIRS})
    target_link_libraries (${PROJECT_NAME} ${GLEW_LIBRARIES})
    add_definitions (-DUSE_GLEW)
endif ()
