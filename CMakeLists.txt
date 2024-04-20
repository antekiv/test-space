cmake_minimum_required(VERSION 3.22.1)

set(PATCH_VERSION "1" CACHE INTERNAL "Patch version")
set(PROJECT_VERSION 0.0.${PATCH_VERSION})

project(test-space VERSION ${PROJECT_VERSION})

configure_file(version.h.in version.h)
set(CMAKE_CXX_STANDARD 17)

add_executable(${CMAKE_PROJECT_NAME} main.cpp)
add_library(lib_space lib.cpp)

target_include_directories(${CMAKE_PROJECT_NAME}
        PRIVATE "${CMAKE_BINARY_DIR}"
)
target_include_directories(lib_space
        PRIVATE "${CMAKE_BINARY_DIR}"
)
target_link_libraries(${CMAKE_PROJECT_NAME} PRIVATE
        lib_space
)

install(TARGETS ${CMAKE_PROJECT_NAME} RUNTIME DESTINATION bin)

set(CPACK_GENERATOR DEB)

set(CPACK_PACKAGE_VERSION_MAJOR "${PROJECT_VERSION_MAJOR}")
set(CPACK_PACKAGE_VERSION_MINOR "${PROJECT_VERSION_MINOR}")
set(CPACK_PACKAGE_VERSION_PATCH "${PROJECT_VERSION_PATCH}")

set(CPACK_PACKAGE_CONTACT antekiv@github.com)

include(CPack)