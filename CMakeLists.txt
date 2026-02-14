cmake_minimum_required(VERSION 3.31)

project(CookbookWrapper)

set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED True)

add_subdirectory(Cookbook)


target_sources(CookbookWrapperLib PRIVATE 
"./Samples/Common_Files/OS.cpp"
"./Samples/Common_Files/Camera.cpp"
"./Samples/Common_Files/CookbookSampleFramework.cpp"
"./Samples/Common_Files/OrbitingCamera.cpp")

if(WIN32)
    
    add_definitions(-DVK_USE_PLATFORM_WIN32_KHR)

    #target_link_libraries(CookbookWrapperLib PUBLIC Cookbook)

endif(WIN32)

if(UNIX)

    add_definitions(-DVK_USE_PLATFORM_XLIB_KHR)
    
    find_package(X11 REQUIRED)

    message(STATUS "X11_FOUND = ${X11_FOUND}")
    message(STATUS "X11_INCLUDE_DIR = ${X11_INCLUDE_DIR}")
    message(STATUS "X11_LIBRARIES = ${X11_LIBRARIES}")

    include_directories(${X11_INCLUDE_DIR})

    link_directories(${X11_LIBRARIES})

    #target_link_libraries(CookbookWrapper ${X11_LIBRARIES} Cookbook) 

endif(UNIX)

target_compile_definitions(CookbookWrapperLib INTERFACE CMAKE_EXPORT_COMPILE_COMMANDS=1)


#target_include_directories(CookbookWrapperLib PRIVATE "./Cookbook" "./Samples/Common_Files" "./Library/Common_Files" "./Library/Source_Files" "./External")