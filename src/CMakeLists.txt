set(SRCS
 forSISLdata.F90
 Curve_Utilities.F90
 Curve_Analysis.F90
 Curve_Definition.F90
 Curve_Interrogation.F90
 Surface_Utilities.F90
 Surface_Analysis.F90
 Surface_Definition.F90
 Surface_Interrogation.F90
 Data_Reduction.F90
 forSISLcurves.F90
 forSISLsurfaces.F90
 forSISLio.F90
 forSISL.F90
 C_utils.c
)

add_library(forsisl ${SRCS})

set_property(TARGET forsisl
  PROPERTY PUBLIC_HEADER 
  ${CMAKE_CURRENT_BINARY_DIR}/forsisldata.mod
 ${CMAKE_CURRENT_BINARY_DIR}/curve_utilities.mod
 ${CMAKE_CURRENT_BINARY_DIR}/curve_analysis.mod
 ${CMAKE_CURRENT_BINARY_DIR}/curve_definition.mod
 ${CMAKE_CURRENT_BINARY_DIR}/curve_interrogation.mod
 ${CMAKE_CURRENT_BINARY_DIR}/surface_utilities.mod
 ${CMAKE_CURRENT_BINARY_DIR}/surface_analysis.mod
 ${CMAKE_CURRENT_BINARY_DIR}/surface_definition.mod
 ${CMAKE_CURRENT_BINARY_DIR}/surface_interrogation.mod
 ${CMAKE_CURRENT_BINARY_DIR}/data_reduction.mod
 ${CMAKE_CURRENT_BINARY_DIR}/forsislcurves.mod
 ${CMAKE_CURRENT_BINARY_DIR}/forsislsurfaces.mod
 ${CMAKE_CURRENT_BINARY_DIR}/forsislio.mod
 ${CMAKE_CURRENT_BINARY_DIR}/forsisl.mod
 )


target_include_directories(forsisl
  PRIVATE ${CMAKE_CURRENT_BINARY_DIR}
  INTERFACE $<INSTALL_INTERFACE:include>
            $<BUILD_INTERFACE:${CMAKE_CURRENT_BINARY_DIR}>
)
target_link_libraries(forsisl PUBLIC
	  sisl
)
install(TARGETS forsisl
  EXPORT ForSISLTargets
  ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
  PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
)

include(GNUInstallDirs)

install(
  EXPORT ForSISLTargets
  DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/forsisl
  NAMESPACE ForSISL::
)