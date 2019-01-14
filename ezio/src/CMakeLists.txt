

add_library(ezio  
    ezio/acceptor.cpp
    ezio/buffer.cpp
    ezio/connector_base.cpp
    ezio/event_loop.cpp
    ezio/event_pump.cpp
    ezio/io_service_context.cpp
    ezio/notifier.cpp
    ezio/socket_address.cpp
    ezio/socket_utils.cpp
    ezio/tcp_server.cpp
    ezio/tcp_client.cpp
    ezio/tcp_connection.cpp
    ezio/timer.cpp
    ezio/timer_queue.cpp
    ezio/this_thread.cpp
    ezio/thread.cpp
    ezio/worker_pool.cpp 
)


target_include_directories(ezio
    PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}
)

if(WIN32)
    target_sources(ezio PRIVATE    
        ezio/acceptor_win.cpp
        ezio/connector_win.cpp
        ezio/event_pump_impl_win.cpp
        ezio/notifier_win.cpp
        ezio/socket_utils_win.cpp 
        ezio/tcp_connection_win.cpp
        ezio/winsock_context.cpp
    )
endif()

if(UNIX)
    target_sources(ezio PRIVATE    
        ezio/acceptor_posix.cpp
        ezio/buffer_posix.cpp
        ezio/connector_posix.cpp
        ezio/event_pump_impl_posix.cpp
        ezio/ignore_sigpipe.cpp
        ezio/notifier_posix.cpp
        ezio/socket_utils_posix.cpp 
        ezio/tcp_connection_posix.cpp
    )
endif()
target_link_libraries(ezio PUBLIC  KBase::KBase)