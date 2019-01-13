
add_library(kbase  
    kbase/at_exit_manager.cpp
    kbase/base64.cpp
    kbase/command_line.cpp
    kbase/chrono_util.cpp
    kbase/error_exception_util.cpp
    kbase/file_iterator.cpp
    kbase/file_util.cpp
    kbase/guid.cpp
    kbase/logging.cpp
    kbase/md5.cpp
    kbase/os_info.cpp
    kbase/path.cpp
    kbase/path_service.cpp
    kbase/pickle.cpp
    kbase/string_encoding_conversions.cpp
    kbase/string_format.cpp
    kbase/string_util.cpp
)


target_include_directories(kbase
    PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}
)

if(WIN32)
    target_sources(kbase PRIVATE    
        kbase/base_path_provider_win.cpp
        kbase/debugger_win.cpp
        kbase/environment_win.cpp
        kbase/file_iterator_win.cpp
        kbase/file_util_win.cpp
        kbase/os_info_win.cpp
        kbase/stack_walker_win.cpp
    )
    target_compile_definitions(kbase 
        PUBLIC
            -DNOMINMAX 
            -DUNICODE 
            -DWIN32_LEAN_AND_MEAN
    )
endif()

if(UNIX)
    target_sources(kbase PRIVATE    
        kbase/base_path_provider_posix.cpp 
        kbase/debugger_posix.cpp 
        kbase/environment_posix.cpp 
        kbase/file_iterator_posix.cpp 
        kbase/file_util_posix.cpp 
        kbase/os_info_posix.cpp 
        kbase/stack_walker_posix.cpp
    )
endif()

set_target_properties(kbase PROPERTIES COTIRE_CXX_PREFIX_HEADER_INIT "pch/precompile.h")
cotire(kbase)
