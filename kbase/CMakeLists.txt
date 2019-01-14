cmake_minimum_required(VERSION 3.5)
project(KBase VERSION 1.0.0 LANGUAGES CXX)
include(cotire.cmake)

add_library(KBase  
    src/kbase/at_exit_manager.cpp
    src/kbase/base64.cpp
    src/kbase/command_line.cpp
    src/kbase/chrono_util.cpp
    src/kbase/error_exception_util.cpp
    src/kbase/file_iterator.cpp
    src/kbase/file_util.cpp
    src/kbase/guid.cpp
    src/kbase/logging.cpp
    src/kbase/md5.cpp
    src/kbase/os_info.cpp
    src/kbase/path.cpp
    src/kbase/path_service.cpp
    src/kbase/pickle.cpp
    src/kbase/string_encoding_conversions.cpp
    src/kbase/string_format.cpp
    src/kbase/string_util.cpp
)

if(WIN32)
    target_sources(KBase PRIVATE    
        src/kbase/base_path_provider_win.cpp
        src/kbase/debugger_win.cpp
        src/kbase/environment_win.cpp
        src/kbase/file_iterator_win.cpp
        src/kbase/file_util_win.cpp
        src/kbase/os_info_win.cpp
        src/kbase/stack_walker_win.cpp
    )
elseif(UNIX)
   target_sources(KBase PRIVATE
        src/kbase/base_path_provider_posix.cpp 
        src/kbase/debugger_posix.cpp 
        src/kbase/environment_posix.cpp 
        src/kbase/file_iterator_posix.cpp 
        src/kbase/file_util_posix.cpp 
        src/kbase/os_info_posix.cpp 
        src/kbase/stack_walker_posix.cpp
    )
endif()

target_include_directories(KBase
    PUBLIC 
        $<INSTALL_INTERFACE:include>
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}/src
)

if(WIN32)
    target_compile_definitions(KBase 
        PUBLIC
            -DNOMINMAX 
            -DUNICODE 
            -DWIN32_LEAN_AND_MEAN
    )
endif()

set_target_properties(KBase PROPERTIES COTIRE_CXX_PREFIX_HEADER_INIT "src/pch/precompile.h")
cotire(KBase)

install(TARGETS KBase
    EXPORT  KBaseTargets
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
    RUNTIME DESTINATION bin
    INCLUDES DESTINATION include
)

set_target_properties(KBase PROPERTIES EXPORT_NAME KBase)

install(DIRECTORY include/ DESTINATION include)

install(EXPORT KBaseTargets
    FILE
        KBaseTargets.cmake 
    NAMESPACE
        KBase::
    DESTINATION
        lib/cmake/KBase
)

#Create a ConfigVersion.cmake file
include(CMakePackageConfigHelpers)
write_basic_package_version_file(
    ${CMAKE_CURRENT_BINARY_DIR}/KBaseConfigVersion.cmake
    VERSION ${PROJECT_VERSION}
    COMPATIBILITY AnyNewerVersion
)

configure_package_config_file(${CMAKE_CURRENT_LIST_DIR}/cmake/KBaseConfig.cmake.in
    ${CMAKE_CURRENT_BINARY_DIR}/KBaseConfig.cmake
    INSTALL_DESTINATION lib/cmake/KBase
)

#Install the config, configversion and custom find modules
install(FILES
    ${CMAKE_CURRENT_BINARY_DIR}/KBaseConfig.cmake
    ${CMAKE_CURRENT_BINARY_DIR}/KBaseConfigVersion.cmake
    DESTINATION lib/cmake/KBase
)

##############################################
#export(EXPORT KBaseTargets FILE ${CMAKE_CURRENT_BINARY_DIR}/KBaseTargets.cmake  NAMESPACE KBase::)

#Register package in user's package registry
#export(PACKAGE KBase)