cmake_minimum_required(VERSION 3.0)
project(PARMETIS VERSION 3.2.0 LANGUAGES C)

option(BUILD_TESTS "${PROJECT_NAME} - Build tests" ON)

find_package(MPI REQUIRED)

# List of directories that cmake will look for CMakeLists.txt
add_subdirectory(METISLib)
add_subdirectory(ParMETISLib)

install(FILES parmetis.h DESTINATION include/parmetis-${PARMETIS_VERSION})

# testing / programs
if (BUILD_TESTS)
#    add_subdirectory(Programs)
endif()