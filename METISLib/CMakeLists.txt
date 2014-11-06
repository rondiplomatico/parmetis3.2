cmake_minimum_required (VERSION 3.0)
PROJECT(METIS VERSION 4.0 LANGUAGES C)

file(GLOB metis_sources *.c)

# Build libmetis.
add_library(metis STATIC ${metis_sources})
target_include_directories(metis PRIVATE ${MPI_C_INCLUDE_PATH} ${MPI_Fortran_INCLUDE_PATH})
set_target_properties(metis PROPERTIES OUTPUT_NAME metis-${METIS_VERSION})
target_include_directories(metis PUBLIC 
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/..>)

if(UNIX)
  target_link_libraries(metis PUBLIC m)
endif()

# Export for both metis and parmetis packages
install(TARGETS metis EXPORT metis-config
    DESTINATION lib
    INCLUDES DESTINATION include/metis-${METIS_VERSION})
install(TARGETS metis EXPORT parmetis-config
    DESTINATION lib
    INCLUDES DESTINATION include/metis-${METIS_VERSION})
file(GLOB headers "*.h")
install(FILES ${headers} ../parmetis.h DESTINATION include/metis-${METIS_VERSION})
install(EXPORT metis-config DESTINATION lib/cmake/metis-${METIS_VERSION})
include(CMakePackageConfigHelpers)
WRITE_BASIC_PACKAGE_VERSION_FILE(${CMAKE_CURRENT_BINARY_DIR}/metis-config-version.cmake COMPATIBILITY AnyNewerVersion)
install(FILES ${CMAKE_CURRENT_BINARY_DIR}/metis-config-version.cmake
    DESTINATION lib/cmake/metis-${METIS_VERSION})