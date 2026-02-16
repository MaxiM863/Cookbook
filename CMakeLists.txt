cmake_minimum_required(VERSION 3.10)

project(CookbookWrapperLib)
set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Define a library target
add_library(CookbookWrapperLib STATIC

"Library/Source_Files/01_Instance_and_Devices/compacte.cpp"
"Library/Source_Files/01_Instance_and_Devices/compacte.h"

"Library/Source_Files/02_Image_Presentation/compacte.cpp"
"Library/Source_Files/02_Image_Presentation/compacte.h"

"Library/Source_Files/03_Command_Buffers_and_Synchronization/compacte.cpp"
"Library/Source_Files/03_Command_Buffers_and_Synchronization/compacte.h"

"Library/Source_Files/04_Resources_and_Memory/compacte.cpp"
"Library/Source_Files/04_Resources_and_Memory/compacte.h"

"Library/Source_Files/05_Descriptor_Sets/compacte.cpp"
"Library/Source_Files/05_Descriptor_Sets/compacte.h"

"Library/Source_Files/06_Render_Passes_and_Framebuffers/compacte.cpp"
"Library/Source_Files/06_Render_Passes_and_Framebuffers/compacte.h"

"Library/Source_Files/08_Graphics_and_Compute_Pipelines/compacte.cpp"
"Library/Source_Files/08_Graphics_and_Compute_Pipelines/compacte.h"

"Library/Source_Files/09_Command_Recording_and_Drawing/compacte.cpp"
"Library/Source_Files/09_Command_Recording_and_Drawing/compacte.h"

"Library/Source_Files/10_Helper_Recipes/compacte.cpp"
"Library/Source_Files/10_Helper_Recipes/compacte.h"

"Library/Common_Files/VulkanFunctions.h"
"Library/Common_Files/VulkanFunctions.cpp"
"Library/Common_Files/AllHeaders.h"
"Library/Common_Files/Common.cpp"
"Library/Common_Files/Common.h"
"Library/Common_Files/ListOfVulkanFunctions.inl"
"Library/Common_Files/Tools.cpp"
"Library/Common_Files/Tools.h"
"Library/Common_Files/VulkanDestroyer.h"

"Samples/Common_Files/OS.cpp"
"Samples/Common_Files/Camera.cpp"
"Samples/Common_Files/CookbookSampleFramework.cpp"
"Samples/Common_Files/OrbitingCamera.cpp"

)

if(WIN32)

    add_definitions(-DVK_USE_PLATFORM_WIN32_KHR)

endif(WIN32)

if(UNIX)

    add_definitions(-DVK_USE_PLATFORM_XLIB_KHR)
    
    find_package(X11 REQUIRED)

    message(STATUS "X11_FOUND = ${X11_FOUND}")
    message(STATUS "X11_INCLUDE_DIR = ${X11_INCLUDE_DIR}")
    message(STATUS "X11_LIBRARIES = ${X11_LIBRARIES}")

    include_directories(${X11_INCLUDE_DIR})

    link_directories(${X11_LIBRARIES})

endif(UNIX)

set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED True)
set_target_properties(CookbookWrapperLib PROPERTIES LINKER_LANGUAGE CXX)

target_compile_definitions(CookbookWrapperLib PUBLIC CMAKE_EXPORT_COMPILE_COMMANDS=1)

target_include_directories(CookbookWrapperLib PUBLIC
$<BUILD_INTERFACE:${X11_INCLUDE_DIR}>
$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/Library/Source_Files>
$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/Library/Common_Files>
$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/External>
$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/Samples/Common_Files>
$<INSTALL_INTERFACE:${X11_INCLUDE_DIR}>
$<INSTALL_INTERFACE:/Library/Source_Files> # relative to install prefix
$<INSTALL_INTERFACE:/Library/Common_Files> # relative to install prefix
$<INSTALL_INTERFACE:/External> # relative to install prefix
$<INSTALL_INTERFACE:/Samples/Common_Files> # relative to install prefix
)

install(TARGETS CookbookWrapperLib
    EXPORT CookbookWrapperLibTargets
    RUNTIME DESTINATION bin
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
)
# Export the target to a file
export(     
EXPORT CookbookWrapperLibTargets
FILE "${CMAKE_CURRENT_BINARY_DIR}/CookbookWrapperLibTargets.cmake"
NAMESPACE CookbookWrapperLib::


)

# Install the export file
install(EXPORT CookbookWrapperLibTargets
    FILE CookbookWrapperLibTargets.cmake
    DESTINATION lib/cmake/CookbookWrapperLib
)