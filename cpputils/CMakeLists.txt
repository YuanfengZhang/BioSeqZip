# ===========================================================================
# CMakeLists.txt
# --------------
#
# Top-level CMake build configuration file. It defines project meta-data
# and decides what sub directories have to be considered for the
# configuration process.
# ---------------------------------------------------------------------------
#
# author : Emanuele Parisi
# ===========================================================================

cmake_minimum_required(VERSION
                       3.7.2)

# ---------------------------------------------------------------------------
# Initialize project configuration.
# ---------------------------------------------------------------------------

project(CppUtils
        LANGUAGES CXX
        VERSION 0.1.2.0)
set(CppUtils_DESCRIPTION
    A set of C++ classes and functions per performing utility tasks.)
set(CppUtils_HOMEPAGE_URL
    http://philae.polito.it/gitlab/parisi/cpputils)
set(CMAKE_CXX_STANDARD
    14)
set(CMAKE_CXX_STANDARD_REQUIRED
    YES)

# ---------------------------------------------------------------------------
# Select the proper default build type.
# ---------------------------------------------------------------------------

if (NOT CMAKE_BUILD_TYPE)
	set(CMAKE_BUILD_TYPE
	    "Release"
	    CACHE STRING
	    "The default project build type is 'Release'")
endif ()
message(STATUS
        "CMake build type is ${CMAKE_BUILD_TYPE}")

# ---------------------------------------------------------------------------
# Configure CppUtils project configuration.
# ---------------------------------------------------------------------------

option(CppUtils_BUILD_TESTS
       "Choose if tests have to be configured" OFF)
option(CppUtils_BUILD_DOCS
       "Choose if documentation has to be configured" OFF)
option(CppUtils_ENABLE_INSTALL
       "Choose if install instructions are configured" ON)

# ---------------------------------------------------------------------------
# Configure CppUtils library targets.
# ---------------------------------------------------------------------------

add_subdirectory(include)

# ---------------------------------------------------------------------------
# Check what of the optional CppUtils project targets have to be
# configured.
# ---------------------------------------------------------------------------

if (${CppUtils_BUILD_TESTS})
	add_subdirectory(tests)
else ()
	message(STATUS
	        "User chose not to build tests")
endif ()

if (${CppUtils_BUILD_DOCS})
	add_subdirectory(docs)
else ()
	message(STATUS
	        "User chose not to build documentation")
endif ()

# ---------------------------------------------------------------------------
# Configure install procedures.
# ---------------------------------------------------------------------------

if (${CppUtils_ENABLE_INSTALL})
	include(GNUInstallDirs)
	include(CMakePackageConfigHelpers)

	set(CppUtils_CONFIG_VERSION_MODULE_PATH
	    ${CMAKE_BINARY_DIR}/CppUtilsConfigVersion.cmake)
	write_basic_package_version_file(${CppUtils_CONFIG_VERSION_MODULE_PATH}
	                                 COMPATIBILITY
	                                 SameMajorVersion)
	install(EXPORT
	        CppUtilsExport
	        DESTINATION
	        ${CMAKE_INSTALL_LIBDIR}/cmake/CppUtils
	        FILE
	        CppUtilsConfig.cmake
	        NAMESPACE
	        CppUtils::)
	install(FILES
	        ${CMAKE_BINARY_DIR}/CppUtilsConfigVersion.cmake
	        DESTINATION
	        ${CMAKE_INSTALL_LIBDIR}/cmake/CppUtils)
	install(DIRECTORY
	        ${CMAKE_CURRENT_SOURCE_DIR}/include/cpputils
	        DESTINATION
	        ${CMAKE_INSTALL_INCLUDEDIR}
	        FILES_MATCHING
	        PATTERN
	        *.h)
else ()
	message(STATUS
	        "User chose not to enable CppUtils installation")
endif ()
