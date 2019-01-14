cmake_minimum_required(VERSION 3.5)
project(EZIO VERSION 1.0.0 LANGUAGES CXX)

list(INSERT CMAKE_PREFIX_PATH 0  ../install)
find_package(KBase 1.0.0 REQUIRED)

add_library(EZIO  
    src/ezio/acceptor.cpp
    src/ezio/buffer.cpp
    src/ezio/connector_base.cpp
    src/ezio/event_loop.cpp
    src/ezio/event_pump.cpp
    src/ezio/io_service_context.cpp
    src/ezio/notifier.cpp
    src/ezio/socket_address.cpp
    src/ezio/socket_utils.cpp
    src/ezio/tcp_server.cpp
    src/ezio/tcp_client.cpp
    src/ezio/tcp_connection.cpp
    src/ezio/timer.cpp
    src/ezio/timer_queue.cpp
    src/ezio/this_thread.cpp
    src/ezio/thread.cpp
    src/ezio/worker_pool.cpp 
)

if(WIN32)
    target_sources(EZIO PRIVATE    
        src/ezio/acceptor_win.cpp
        src/ezio/connector_win.cpp
        src/ezio/event_pump_impl_win.cpp
        src/ezio/notifier_win.cpp
        src/ezio/socket_utils_win.cpp 
        src/ezio/tcp_connection_win.cpp
        src/ezio/winsock_context.cpp
    )
elseif(UNIX)
    target_sources(EZIO PRIVATE    
        src/ezio/acceptor_posix.cpp
        src/ezio/buffer_posix.cpp
        src/ezio/connector_posix.cpp
        src/ezio/event_pump_impl_posix.cpp
        src/ezio/ignore_sigpipe.cpp
        src/ezio/notifier_posix.cpp
        src/ezio/socket_utils_posix.cpp 
        src/ezio/tcp_connection_posix.cpp
    )
endif()

target_include_directories(EZIO
    PUBLIC
        $<INSTALL_INTERFACE:include>
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}/src
)

target_link_libraries(EZIO PUBLIC  KBase::KBase)


install(TARGETS EZIO
    EXPORT  EZIOTargets
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
    RUNTIME DESTINATION bin
    INCLUDES DESTINATION include
)

set_target_properties(EZIO PROPERTIES EXPORT_NAME EZIO)

install(DIRECTORY include/ DESTINATION include)


install(EXPORT EZIOTargets
    FILE
        EZIOTargets.cmake 
    NAMESPACE
        EZIO::
    DESTINATION
        lib/cmake/EZIO
)



include(CMakePackageConfigHelpers)
write_basic_package_version_file(
    ${CMAKE_CURRENT_BINARY_DIR}/EZIOConfigVersion.cmake
    VERSION ${PROJECT_VERSION}
    COMPATIBILITY AnyNewerVersion
)

configure_package_config_file(${CMAKE_CURRENT_LIST_DIR}/cmake/EZIOConfig.cmake.in
    ${CMAKE_CURRENT_BINARY_DIR}/EZIOConfig.cmake
    INSTALL_DESTINATION lib/cmake/EZIO
)

#Install the config, configversion and custom find modules
install(FILES
    ${CMAKE_CURRENT_BINARY_DIR}/EZIOConfig.cmake
    ${CMAKE_CURRENT_BINARY_DIR}/EZIOConfigVersion.cmake
    DESTINATION lib/cmake/EZIO
)
