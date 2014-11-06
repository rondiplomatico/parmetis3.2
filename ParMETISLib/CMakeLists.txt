SET(metis_sources comm.c util.c debug.c setup.c grsetup.c timer.c 
       node_refine.c initmsection.c 
       xyzpart.c pspases.c frename.c 
       iintsort.c iidxsort.c ikeysort.c ikeyvalsort.c 
       kmetis.c gkmetis.c ometis.c 
       initpart.c match.c 
       kwayfm.c kwayrefine.c kwaybalance.c 
       remap.c stat.c fpqueue.c 
       ametis.c rmetis.c initbalance.c 
       mdiffusion.c diffutil.c wave.c 
       csrmatch.c redomylink.c balancemylink.c 
       selectq.c akwayfm.c serial.c move.c 
       mmetis.c mesh.c memory.c weird.c backcompat.c
)

# Build libmetis.
add_library(parmetis STATIC ${metis_sources})
target_link_libraries(parmetis PUBLIC metis)
target_include_directories(parmetis PRIVATE ${MPI_C_INCLUDE_PATH} ${MPI_Fortran_INCLUDE_PATH})
set_target_properties(parmetis PROPERTIES OUTPUT_NAME parmetis-${PARMETIS_VERSION})
target_include_directories(parmetis PUBLIC $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>)

if(UNIX)
  target_link_libraries(parmetis PUBLIC m)
endif()

# Export for both metis and parmetis packages
install(TARGETS parmetis EXPORT parmetis-config
    DESTINATION lib
    INCLUDES DESTINATION include/parmetis-${PARMETIS_VERSION})
#file(GLOB headers "*.h")
install(FILES ${headers} DESTINATION include/parmetis-${PARMETIS_VERSION})
install(EXPORT parmetis-config DESTINATION lib/cmake/parmetis-${PARMETIS_VERSION})
include(CMakePackageConfigHelpers)
WRITE_BASIC_PACKAGE_VERSION_FILE(${CMAKE_CURRENT_BINARY_DIR}/parmetis-config-version.cmake COMPATIBILITY AnyNewerVersion)
install(FILES ${CMAKE_CURRENT_BINARY_DIR}/parmetis-config-version.cmake
    DESTINATION lib/cmake/parmetis-${PARMETIS_VERSION})