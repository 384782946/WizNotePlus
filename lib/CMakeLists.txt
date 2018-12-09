project(libs)

include(ExternalProject)
# find_package(ZLIB)
# if (NOT ZLIB_FOUND)
#     # use conan zlib package
#     find_package(zlib REQUIRED)
#     set(ZLIB_INCLUDE_DIR ${zlib_INCLUDE_DIRS})
#     set(ZLIB_LIBRARY ${zlib_LIBRARIES})
# endif()

# if (NOT EXISTS ${CMAKE_SOURCE_DIR}/lib/quazip/quazip/quazip.h)
#     # Update submodules as needed
#     message("QuaZip not found, it needs to be downloaded from github and build from source.")
#     message("Start download: ")
#     execute_process(COMMAND git submodule update --init -- quazip
#                 WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
#                 RESULT_VARIABLE GIT_SUBMOD_RESULT)
#     if(NOT GIT_SUBMOD_RESULT EQUAL "0")
#         message(FATAL_ERROR "git submodule update --init failed with ${GIT_SUBMOD_RESULT}, please checkout submodules")
#     endif()
#     # build from source.
#     add_subdirectory(quazip)
# else()
#     add_subdirectory(quazip)
# endif()

ExternalProject_Add(quazip
  GIT_REPOSITORY https://github.com/altairwei/quazip.git
  GIT_TAG        7dea8a4d70690df854b5a806cae8f1e5fa2434fa
  STEP_TARGETS   build
)

#add_subdirectory(aggregation)
#add_subdirectory(extensionsystem)

if (NOT EXISTS ${CMAKE_SOURCE_DIR}/lib/gumbo-parser/src/gumbo.h)
    # Update submodules as needed
    message("Gumbo not found, it needs to be downloaded from github and build from source.")
    message("Start download: ")
    execute_process(COMMAND git submodule update --init -- gumbo-parser
                WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
        RESULT_VARIABLE GIT_SUBMOD_RESULT)
    if(NOT GIT_SUBMOD_RESULT EQUAL "0")
        message(FATAL_ERROR "git submodule update --init failed with ${GIT_SUBMOD_RESULT}, please checkout submodules")
    endif()
    # build from source.
    set(Gumbo_INCLUDE_DIR ${CMAKE_SOURCE_DIR}/lib/gumbo-parser/src/)
    add_subdirectory(gumbo-parser)
    set(Gumbo_static_LIBRARY Gumbo_static)
    set(Gumbo_LIBRARY Gumbo)
else()
    set(Gumbo_INCLUDE_DIR ${CMAKE_SOURCE_DIR}/lib/gumbo-parser/src/)
    add_subdirectory(gumbo-parser)
    set(Gumbo_static_LIBRARY Gumbo_static)
    set(Gumbo_LIBRARY Gumbo)
endif()

if (NOT EXISTS ${CMAKE_SOURCE_DIR}/lib/gumbo-query/src/Document.h)
    # Update submodules as needed
    message("GumboQuery not found, it needs to be downloaded from github and build from source.")
    message("Start download: ")
    execute_process(COMMAND git submodule update --init -- gumbo-query
                WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
        RESULT_VARIABLE GIT_SUBMOD_RESULT)
    if(NOT GIT_SUBMOD_RESULT EQUAL "0")
        message(FATAL_ERROR "git submodule update --init failed with ${GIT_SUBMOD_RESULT}, please checkout submodules")
    endif()
    # build from source.
    add_subdirectory(gumbo-query)
else()
    add_subdirectory(gumbo-query)
endif()
