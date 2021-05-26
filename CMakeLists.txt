project(GeometryCentral)

cmake_minimum_required(VERSION 3.14)

set(PROJECT_VERSION_MAJOR 1)
set(PROJECT_VERSION_MINOR 0)
set(PROJECT_VERSION_PATCH 0)

# Find includes in the build directories
set(CMAKE_INCLUDE_CURRENT_DIR ON)

include(FindOpenMP)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -O2 -ftree-vectorize -fPIC -std=c++11 ${OpenMP_C_FLAGS}")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O2 -ftree-vectorize -fPIC -std=c++11 ${OpenMP_CXX_FLAGS}")
set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -O2 -ftree-vectorize -fPIC -flto -std=c++11 ${OpenMP_EXE_LINKER_FLAGS}")
set(CMAKE_INSTALL_RPATH "${CMAKE_INSTALL_PREFIX}/lib")
set(CMAKE_INSTALL_RPATH_USE_LINK_PATH TRUE)
add_definitions(-DNGC_SAFETY_CHECKS)


#================================
# Dependencies
#================================

include_directories(deps)
include_directories(deps/happly)
include_directories(deps/nanoflann)
include_directories(deps/nanort)
include_directories(deps/Eigen)

#================================
# Building
#================================
set(BUILD_SHARED_LIBS ON)

set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

include_directories(src)
include_directories(src/numerical)
include_directories(src/pointcloud)
include_directories(src/surface)
include_directories(src/utilities)
include_directories(include)
include_directories(include/geometrycentral/numerical)
include_directories(include/geometrycentral/pointcloud)
include_directories(include/geometrycentral/surface)
include_directories(include/geometrycentral/utilities)


set(SRC_ROOT "./src/")
set(SRCS
  ${SRC_ROOT}/surface/surface_mesh.cpp
  ${SRC_ROOT}/surface/manifold_surface_mesh.cpp
  ${SRC_ROOT}/surface/halfedge_factories.cpp  
  ${SRC_ROOT}/surface/surface_mesh_factories.cpp
  ${SRC_ROOT}/surface/meshio.cpp
  ${SRC_ROOT}/surface/simple_polygon_mesh.cpp
  ${SRC_ROOT}/surface/rich_surface_mesh_data.cpp

  ${SRC_ROOT}/surface/base_geometry_interface.cpp
  ${SRC_ROOT}/surface/intrinsic_geometry_interface.cpp
  ${SRC_ROOT}/surface/extrinsic_geometry_interface.cpp
  ${SRC_ROOT}/surface/embedded_geometry_interface.cpp
  ${SRC_ROOT}/surface/edge_length_geometry.cpp
  ${SRC_ROOT}/surface/vertex_position_geometry.cpp
  ${SRC_ROOT}/surface/direction_fields.cpp
  ${SRC_ROOT}/surface/heat_method_distance.cpp
  ${SRC_ROOT}/surface/vector_heat_method.cpp
  ${SRC_ROOT}/surface/trace_geodesic.cpp
  ${SRC_ROOT}/surface/surface_centers.cpp
  ${SRC_ROOT}/surface/signpost_intrinsic_triangulation.cpp
  ${SRC_ROOT}/surface/simple_idt.cpp
  ${SRC_ROOT}/surface/mesh_graph_algorithms.cpp
  ${SRC_ROOT}/surface/surface_point.cpp
  ${SRC_ROOT}/surface/fast_marching_method.cpp
  ${SRC_ROOT}/surface/uniformize.cpp
  ${SRC_ROOT}/surface/parameterize.cpp
  ${SRC_ROOT}/surface/surgery.cpp
  ${SRC_ROOT}/surface/simple_idt.cpp
  ${SRC_ROOT}/surface/exact_polyhedral_geodesics.cpp
  ${SRC_ROOT}/surface/intrinsic_mollification.cpp
  ${SRC_ROOT}/surface/tufted_laplacian.cpp
  ${SRC_ROOT}/surface/flip_geodesics.cpp

  ${SRC_ROOT}/numerical/linear_algebra_utilities.cpp
  ${SRC_ROOT}/numerical/suitesparse_utilities.cpp
  ${SRC_ROOT}/numerical/linear_solvers.cpp
  ${SRC_ROOT}/numerical/eigenproblem_solvers.cpp
  ${SRC_ROOT}/numerical/qr_solvers.cpp
  ${SRC_ROOT}/numerical/square_solvers.cpp
  ${SRC_ROOT}/numerical/positive_definite_solvers.cpp

  ${SRC_ROOT}/utilities/utilities.cpp
  ${SRC_ROOT}/utilities/quaternion.cpp
  ${SRC_ROOT}/utilities/disjoint_sets.cpp
  ${SRC_ROOT}/utilities/knn.cpp
)

set(INCLUDE_ROOT "./include/geometrycentral/")
set(HEADERS
  ${INCLUDE_ROOT}/numerical/linear_algebra_utilities.h
  ${INCLUDE_ROOT}/numerical/linear_algebra_utilities.ipp
  ${INCLUDE_ROOT}/numerical/linear_solvers.h
  ${INCLUDE_ROOT}/numerical/suitesparse_utilities.h

  ${INCLUDE_ROOT}/surface/barycentric_coordinate_helpers.h
  ${INCLUDE_ROOT}/surface/barycentric_coordinate_helpers.ipp
  ${INCLUDE_ROOT}/surface/base_geometry_interface.h
  ${INCLUDE_ROOT}/surface/detect_symmetry.h
  ${INCLUDE_ROOT}/surface/direction_fields.h
  ${INCLUDE_ROOT}/surface/edge_length_geometry.h
  ${INCLUDE_ROOT}/surface/edge_length_geometry.ipp
  ${INCLUDE_ROOT}/surface/embedded_geometry_interface.h
  ${INCLUDE_ROOT}/surface/exact_polyhedral_geodesics.h
  ${INCLUDE_ROOT}/surface/extrinsic_geometry_interface.h
  ${INCLUDE_ROOT}/surface/fast_marching_method.h
  ${INCLUDE_ROOT}/surface/halfedge_element_types.h
  ${INCLUDE_ROOT}/surface/halfedge_element_types.ipp
  ${INCLUDE_ROOT}/surface/halfedge_factories.h
  ${INCLUDE_ROOT}/surface/halfedge_logic_templates.ipp
  ${INCLUDE_ROOT}/surface/halfedge_mesh.h
  ${INCLUDE_ROOT}/surface/heat_method_distance.h
  ${INCLUDE_ROOT}/surface/intrinsic_geometry_interface.h
  ${INCLUDE_ROOT}/surface/intrinsic_mollification.h
  ${INCLUDE_ROOT}/surface/manifold_surface_mesh.h
  ${INCLUDE_ROOT}/surface/meshio.h
  ${INCLUDE_ROOT}/surface/mesh_graph_algorithms.h
  ${INCLUDE_ROOT}/surface/mesh_ray_tracer.h
  ${INCLUDE_ROOT}/surface/parameterize.h
  ${INCLUDE_ROOT}/surface/rich_surface_mesh_data.h
  ${INCLUDE_ROOT}/surface/rich_surface_mesh_data.ipp
  ${INCLUDE_ROOT}/surface/polygon_soup_mesh.h
  ${INCLUDE_ROOT}/surface/signpost_intrinsic_triangulation.h
  ${INCLUDE_ROOT}/surface/signpost_intrinsic_triangulation.ipp
  ${INCLUDE_ROOT}/surface/simple_idt.h
  ${INCLUDE_ROOT}/surface/simple_polygon_mesh.h
  ${INCLUDE_ROOT}/surface/surface_centers.h
  ${INCLUDE_ROOT}/surface/surface_mesh.h
  ${INCLUDE_ROOT}/surface/surface_mesh.ipp
  ${INCLUDE_ROOT}/surface/surface_point.h
  ${INCLUDE_ROOT}/surface/surface_point.ipp
  ${INCLUDE_ROOT}/surface/surgery.h
  ${INCLUDE_ROOT}/surface/trace_geodesic.h
  ${INCLUDE_ROOT}/surface/uniformize.h
  ${INCLUDE_ROOT}/surface/vector_heat_method.h
  ${INCLUDE_ROOT}/surface/vertex_position_geometry.h
  ${INCLUDE_ROOT}/surface/vertex_position_geometry.ipp

  ${INCLUDE_ROOT}/utilities/combining_hash_functions.h
  ${INCLUDE_ROOT}/utilities/curve.h
  ${INCLUDE_ROOT}/utilities/curve.ipp
  ${INCLUDE_ROOT}/utilities/dependent_quantity.h
  ${INCLUDE_ROOT}/utilities/dependent_quantity.ipp
  ${INCLUDE_ROOT}/utilities/disjoint_sets.h
  ${INCLUDE_ROOT}/utilities/eigen_interop_helpers.h
  ${INCLUDE_ROOT}/utilities/knn.h
  ${INCLUDE_ROOT}/utilities/mesh_data.h
  ${INCLUDE_ROOT}/utilities/mesh_data.ipp
  ${INCLUDE_ROOT}/utilities/quaternion.h
  ${INCLUDE_ROOT}/utilities/timing.h
  ${INCLUDE_ROOT}/utilities/utilities.h
  ${INCLUDE_ROOT}/utilities/vector2.h
  ${INCLUDE_ROOT}/utilities/vector2.ipp
  ${INCLUDE_ROOT}/utilities/vector3.h
  ${INCLUDE_ROOT}/utilities/vector3.ipp
)

add_library(GeometryCentral ${SRCS} ${HEADERS})

set_target_properties(GeometryCentral PROPERTIES VERSION
    ${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_MINOR}.${PROJECT_VERSION_PATCH})
set_target_properties(GeometryCentral PROPERTIES LINKER_LANGUAGE C)
target_link_libraries(GeometryCentral)

include_directories(include/geometrycentral/numerical)
include_directories(include/geometrycentral/pointcloud)
include_directories(include/geometrycentral/surface)
include_directories(include/geometrycentral/utilities)


#================================
# Install
#================================
include(InstallRequiredSystemLibraries)

# Install libraries
install(TARGETS GeometryCentral DESTINATION ${CMAKE_INSTALL_PREFIX}/lib)

# Install include files
install(DIRECTORY ${CMAKE_SOURCE_DIR}/include/geometrycentral/numerical DESTINATION ${CMAKE_INSTALL_PREFIX}/include)
install(DIRECTORY ${CMAKE_SOURCE_DIR}/include/geometrycentral/pointcloud DESTINATION ${CMAKE_INSTALL_PREFIX}/include)
install(DIRECTORY ${CMAKE_SOURCE_DIR}/include/geometrycentral/surface DESTINATION ${CMAKE_INSTALL_PREFIX}/include)
install(DIRECTORY ${CMAKE_SOURCE_DIR}/include/geometrycentral/utilities DESTINATION ${CMAKE_INSTALL_PREFIX}/include)
install(DIRECTORY ${CMAKE_SOURCE_DIR}/deps DESTINATION ${CMAKE_INSTALL_PREFIX}/include)
install(DIRECTORY ${CMAKE_SOURCE_DIR}/deps/happly DESTINATION ${CMAKE_INSTALL_PREFIX}/include)
install(DIRECTORY ${CMAKE_SOURCE_DIR}/deps/nanoflann DESTINATION ${CMAKE_INSTALL_PREFIX}/include)
install(DIRECTORY ${CMAKE_SOURCE_DIR}/deps/nanort DESTINATION ${CMAKE_INSTALL_PREFIX}/include)
install(DIRECTORY ${CMAKE_SOURCE_DIR}/deps/Eigen DESTINATION ${CMAKE_INSTALL_PREFIX}/include)
