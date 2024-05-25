cmake_minimum_required(VERSION 3.22.1)

set(PATCH_VERSION "1" CACHE INTERNAL "Patch version")
set(PROJECT_VERSION 0.0.${PATCH_VERSION})

project(MyProject VERSION ${PROJECT_VERSION}
        DESCRIPTION "Builded using modern CMake"
        LANGUAGES CXX)

find_package(Boost COMPONENTS unit_test_framework REQUIRED)

configure_file(version.h.in version.h)
set(CMAKE_CXX_STANDARD 17)

add_executable(${CMAKE_PROJECT_NAME} main.cpp)
add_library(lib_space lib.cpp)
add_executable(test_version test_version.cpp)

set_target_properties(test_version PROPERTIES
        INCLUDE_DIRECTORIES ${Boost_INCLUDE_DIR}
)
target_include_directories(${CMAKE_PROJECT_NAME}
        PRIVATE "${CMAKE_BINARY_DIR}"
)
target_include_directories(lib_space
        PRIVATE "${CMAKE_BINARY_DIR}"
)
target_link_libraries(${CMAKE_PROJECT_NAME} PRIVATE
        lib_space
)
target_link_libraries(test_version
        ${Boost_LIBRARIES}
        lib_space
)

install(TARGETS ${CMAKE_PROJECT_NAME} RUNTIME DESTINATION bin)

set(CPACK_GENERATOR DEB)

set(CPACK_PACKAGE_VERSION_MAJOR "${PROJECT_VERSION_MAJOR}")
set(CPACK_PACKAGE_VERSION_MINOR "${PROJECT_VERSION_MINOR}")
set(CPACK_PACKAGE_VERSION_PATCH "${PROJECT_VERSION_PATCH}")

set(CPACK_PACKAGE_CONTACT antekiv@github.com)

include(CPack)

enable_testing()

add_test(test_version test_version)
