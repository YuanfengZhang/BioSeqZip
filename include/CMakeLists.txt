# ===========================================================================
# include/CMakeLists.txt
# ----------------------
#
# Module for creating the BioSeqZip library core target.
# Modified to use local submodules instead of FetchContent.
# ---------------------------------------------------------------------------
#
# author : Emanuele Parisi
# modified by: Yuanfeng Zhang
# ===========================================================================

# ---------------------------------------------------------------------------
# Retrieve the external projects BioSeqZip depends on.
# Using local submodules instead of FetchContent for offline builds.
# ---------------------------------------------------------------------------

# Use local SeqAn from submodule (at ../seqan relative to BioSeqZip)
set(bioseqzip_seqan_SOURCE_DIR "${CMAKE_SOURCE_DIR}/../seqan")
set(bioseqzip_seqan_POPULATED TRUE)
list(APPEND
     CMAKE_INCLUDE_PATH
     ${CMAKE_INCLUDE_PATH}
     ${bioseqzip_seqan_SOURCE_DIR}/include)
set(SEQAN_FIND_DEPENDENCIES
    "DEFAULT")
find_package(SEQAN
             REQUIRED
             HINTS ${bioseqzip_seqan_SOURCE_DIR}/util/cmake)

add_library(BioSeqZip_SeqAn
            INTERFACE)
target_include_directories(BioSeqZip_SeqAn
                           INTERFACE
                           ${SEQAN_INCLUDE_DIRS})
target_link_libraries(BioSeqZip_SeqAn
                      INTERFACE
                      ${SEQAN_LIBRARIES})
target_compile_definitions(BioSeqZip_SeqAn
                           INTERFACE
                           ${SEQAN_DEFINITIONS}
                           -DSEQAN_DISABLE_VERSION_CHECK=ON)
separate_arguments(SEQAN_SEPARATED_CXX_FLAGS
                   UNIX_COMMAND
                   ${SEQAN_CXX_FLAGS})
target_compile_options(BioSeqZip_SeqAn
                       INTERFACE
                       ${SEQAN_SEPARATED_CXX_FLAGS})

# Use local CppUtilities from submodule (at ../cpputils relative to BioSeqZip)
set(bioseqzip_cpputils_SOURCE_DIR "${CMAKE_SOURCE_DIR}/../cpputils")
set(bioseqzip_cpputils_BINARY_DIR "${CMAKE_BINARY_DIR}/cpputils")
set(bioseqzip_cpputils_POPULATED TRUE)
add_subdirectory(${bioseqzip_cpputils_SOURCE_DIR}
                 ${bioseqzip_cpputils_BINARY_DIR})

# ---------------------------------------------------------------------------
# Configure the library global target BioSeqZip::BioSeqZip.
# ---------------------------------------------------------------------------

add_library(BioSeqZip
            INTERFACE)
target_include_directories(BioSeqZip
                           INTERFACE
                           $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
                           $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>)
target_link_libraries(BioSeqZip
                      INTERFACE
                      BioSeqZip_SeqAn
                      CppUtils
                      pthread
                      stdc++fs
                      -fopenmp)
target_compile_definitions(BioSeqZip
                           INTERFACE
                           -DBioSeqZip_VERSION="${BioSeqZip_VERSION}")
