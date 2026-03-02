# ===========================================================================
# include/CMakeLists.txt
# ----------------------
#
# Module for creatign the CppUtils library target.
# ---------------------------------------------------------------------------
#
# author : Emanuele Parisi
# ===========================================================================

include(GNUInstallDirs)

# ---------------------------------------------------------------------------
# Configure the library global target CppUtils.
# ---------------------------------------------------------------------------

add_library(CppUtils
            INTERFACE)
target_include_directories(CppUtils
                           INTERFACE
                           $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
                           $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>)

# ---------------------------------------------------------------------------
# Configure library target installation.
# ---------------------------------------------------------------------------

if (${CppUtils_ENABLE_INSTALL})
	install(TARGETS
	        CppUtils
	        EXPORT
	        CppUtilsExport
	        DESTINATION
	        ${CMAKE_INSTALL_INCLUDEDIR})
endif ()
