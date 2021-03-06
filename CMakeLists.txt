cmake_minimum_required(VERSION 3.16)
project(HDKCMakeSample)

set(CMAKE_CXX_STANDARD 14)

# CMAKE_PREFIX_PATH must contain the path to the toolkit/cmake subdirectory of
# the Houdini installation. See the "Compiling with CMake" section of the HDK
# documentation for more details, which describes several options for
# specifying this path.
list( APPEND CMAKE_PREFIX_PATH "$ENV{HFS}/toolkit/cmake" )

# Locate Houdini's libraries and header files.
# Registers an imported library target named 'Houdini'.
find_package(Houdini REQUIRED)

#add_executable(geoisosurface exe/geoisosurface.C)
#target_link_libraries(geoisosurface Houdini)
#houdini_configure_target(geoisosurface)

# Add a library with two source files.
set( library_name SOP_Star )

# Code generation for the embedded DS file in SOP_Star.C.
houdini_generate_proto_headers( FILES SOP_Star.C )

add_library( ${library_name} SHARED
        sharedlib/SOP_Star.C
        sharedlib/SOP_Star.h
        )
# Link against the Houdini libraries, and add required include directories and compile definitions.
target_link_libraries( ${library_name} Houdini )

# Include ${CMAKE_CURRENT_BINARY_DIR} for the generated header.
target_include_directories( ${library_name} PRIVATE
        ${CMAKE_CURRENT_BINARY_DIR}
        )

# Configure several common target properties, such as its output directory.
houdini_configure_target( ${library_name} )