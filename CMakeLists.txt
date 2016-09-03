CMAKE_MINIMUM_REQUIRED(VERSION 2.6)

PROJECT(OpenscenegraphThirdParty)

# Only enable release and debug builds
IF(CMAKE_CONFIGURATION_TYPES)
  SET(CMAKE_CONFIGURATION_TYPES Debug Release)
  SET(CMAKE_CONFIGURATION_TYPES "${CMAKE_CONFIGURATION_TYPES}" CACHE STRING
    "Reset the configurations to what we need"
    FORCE)
ENDIF()

# Generate folder name for install
IF(${CMAKE_SIZEOF_VOID_P} EQUAL 8)
  SET(BITS "x64")
ELSEIF (${CMAKE_SIZEOF_VOID_P} EQUAL 4)
  SET(BITS "x86")
ENDIF()

IF (MSVC)
  IF (${MSVC_VERSION} EQUAL 1600)
    SET(COMPILER "v100") # Visual Studio 2010
  ELSEIF (${MSVC_VERSION} EQUAL 1700)
    SET(COMPILER "v110") # Visual Studio 2012
  ELSEIF (${MSVC_VERSION} EQUAL 1800)
    SET(COMPILER "v120") # Visual Studio 2013
  ELSEIF (${MSVC_VERSION} EQUAL 1900)
    SET(COMPILER "v140") # Visual Studio 2015
  ENDIF()
ENDIF()

IF(MINGW)
  SET(COMPILER "mingw")
ENDIF()

SET(dirname "${COMPILER}-${BITS}")

#Set directories
SET(EXECUTABLE_OUTPUT_PATH "${PROJECT_BINARY_DIR}/3rdParty/${dirname}/bin")
SET(ARCHIVE_OUTPUT_PATH "${PROJECT_BINARY_DIR}/3rdParty/${dirname}/lib")
SET(LIBRARY_OUTPUT_PATH "${PROJECT_BINARY_DIR}/lib")
SET(INCLUDE_OUTPUT_PATH "${PROJECT_BINARY_DIR}/3rdParty/${dirname}/include")


SET(ZLIB_SOURCE_DIR CACHE PATH "Path where to find ZLIB source")
SET(LIBPNG_SOURCE_DIR CACHE PATH "Path where to find LIBPNG source")
SET(LIBJPEG_SOURCE_DIR CACHE PATH "Path where to find LIBJPEG source")
SET(LIBTIFF_SOURCE_DIR CACHE PATH "Path where to find LIBTIFF source")
SET(FREETYPE_SOURCE_DIR CACHE PATH "Path where to find FREETYPE source")
SET(GLUT_SOURCE_DIR CACHE PATH "Path where to find GLUT source")
SET(GIFLIB_SOURCE_DIR CACHE PATH "Path where to find GIFLIB source")
SET(MINIZIP_SOURCE_DIR CACHE PATH "Path where to find MINIZIP source")
SET(CURL_SOURCE_DIR CACHE PATH "Path where to find cURL source")

SET(SUBMODULE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/submodules")

MACRO(FIND_SUBMODULE VAR DIR)
  IF(NOT ${VAR} AND EXISTS "${SUBMODULE_DIR}/${DIR}")
    SET(${VAR} "${SUBMODULE_DIR}/${DIR}")
  ENDIF()
ENDMACRO()

FIND_SUBMODULE(ZLIB_SOURCE_DIR "zlib")
FIND_SUBMODULE(LIBPNG_SOURCE_DIR "libpng")
FIND_SUBMODULE(LIBJPEG_SOURCE_DIR "libjpeg")
FIND_SUBMODULE(LIBTIFF_SOURCE_DIR "libtiff")
FIND_SUBMODULE(FREETYPE_SOURCE_DIR "freetype2")
FIND_SUBMODULE(MINIZIP_SOURCE_DIR "minizip")
FIND_SUBMODULE(CURL_SOURCE_DIR "curl")

# Check dependencies for LIBPNG
IF (LIBPNG_SOURCE_DIR)
  IF(NOT ZLIB_SOURCE_DIR)
    MESSAGE(FATAL_ERROR "Error: LIBPNG depends on ZLIB and no ZLIB path has been set")
  ENDIF()
ENDIF ()

# Check dependencies for LIBTIFF
IF(LIBTIFF_SOURCE_DIR)
  IF(NOT ZLIB_SOURCE_DIR)
    MESSAGE(FATAL_ERROR "Error: LIBTIFF depends on ZLIB and no ZLIB path has been set")
  ENDIF()

  IF(NOT LIBJPEG_SOURCE_DIR)
    MESSAGE(FATAL_ERROR "Error: LIBTIFF depends on LIBJPEG and no LIBJPEG path has been set")
  ENDIF()
ENDIF()

# Check dependencies for MINIZIP
IF (MINIZIP_SOURCE_DIR)
  IF(NOT ZLIB_SOURCE_DIR)
    MESSAGE(FATAL_ERROR "Error: MINIZIP depends on ZLIB and no ZLIB path has been set")
  ENDIF()
ENDIF ()

# Add libraries
IF (ZLIB_SOURCE_DIR)
  MESSAGE("[+] zlib found")
  ADD_SUBDIRECTORY(zlib)
ELSE()
  MESSAGE("[ ] zlib not found")
ENDIF()

IF (LIBPNG_SOURCE_DIR)
  MESSAGE("[+] libpng found")
  ADD_SUBDIRECTORY(libpng)
ELSE()
  MESSAGE("[ ] libpng not found")
ENDIF()

IF (LIBJPEG_SOURCE_DIR)
  MESSAGE("[+] libjpeg found")
  ADD_SUBDIRECTORY(libjpeg)
ELSE()
  MESSAGE("[ ] libjpeg not found")
ENDIF()

IF (LIBTIFF_SOURCE_DIR)
  MESSAGE("[+] libtiff found")
  ADD_SUBDIRECTORY(libtiff)
ELSE()
  MESSAGE("[ ] libtiff not found")
ENDIF()

IF (FREETYPE_SOURCE_DIR)
  MESSAGE("[+] freetype found")
  ADD_SUBDIRECTORY(freetype)
ELSE()
  MESSAGE("[ ] freetype not found")
ENDIF()

IF (GLUT_SOURCE_DIR)
  MESSAGE("[+] glut found")
  ADD_SUBDIRECTORY(glut)
ELSE()
  MESSAGE("[ ] glut not found")
ENDIF()

IF (GIFLIB_SOURCE_DIR)
  MESSAGE("[+] giflib found")
  ADD_SUBDIRECTORY(giflib)
ELSE()
  MESSAGE("[ ] giflib not found")
ENDIF()

IF (MINIZIP_SOURCE_DIR)
  MESSAGE("[+] minizip found")
  ADD_SUBDIRECTORY(minizip)
ELSE()
  MESSAGE("[ ] minizip not found")
ENDIF()

IF (CURL_SOURCE_DIR)
  MESSAGE("[+] curl found")
  ADD_SUBDIRECTORY(curl)
ELSE()
  MESSAGE("[ ] curl not found")
ENDIF()
