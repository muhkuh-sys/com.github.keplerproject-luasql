cmake_minimum_required(VERSION 3.2.3)

PROJECT("mariadb-connector-c")

INCLUDE(ExternalProject)

# Python is used for some test scripts.
FIND_PACKAGE(PythonInterp 2.7 REQUIRED)


#----------------------------------------------------------------------------
#
# Build the project.
#
SET(PRJ_CMAKE_ARGS "")

LIST(APPEND PRJ_CMAKE_ARGS "-DWITH_MYSQLCOMPAT=ON")
LIST(APPEND PRJ_CMAKE_ARGS "-DWITH_UNIT_TESTS=OFF")
LIST(APPEND PRJ_CMAKE_ARGS "-DWITH_CURL=OFF")
LIST(APPEND PRJ_CMAKE_ARGS "-DWITH_SSL=GNUTLS")
LIST(APPEND PRJ_CMAKE_ARGS "-DCLIENT_PLUGIN_DIALOG=STATIC")
LIST(APPEND PRJ_CMAKE_ARGS "-DCLIENT_PLUGIN_CLIENT_ED25519=OFF")
LIST(APPEND PRJ_CMAKE_ARGS "-DCLIENT_PLUGIN_CACHING_SHA2_PASSWORD=STATIC")
#LIST(APPEND PRJ_CMAKE_ARGS "-DCLIENT_PLUGIN_SHA256_PASSWORD=STATIC")
#LIST(APPEND PRJ_CMAKE_ARGS "-DCLIENT_PLUGIN_AUTH_GSSAPI_CLIENT=OFF")
#LIST(APPEND PRJ_CMAKE_ARGS "-DCLIENT_PLUGIN_AUTH_GSSAPI=OFF")
LIST(APPEND PRJ_CMAKE_ARGS "-DCLIENT_PLUGIN_MYSQL_CLEAR_PASSWORD=STATIC")
LIST(APPEND PRJ_CMAKE_ARGS "-DGNUTLS_LIBRARY=${CMAKE_BINARY_DIR}/gnutls/install/lib/libgnutls.a")
LIST(APPEND PRJ_CMAKE_ARGS "-DHOGWEED_LIBRARY=${CMAKE_BINARY_DIR}/libnettle/install/lib/libhogweed.a")
LIST(APPEND PRJ_CMAKE_ARGS "-DNETTLE_LIBRARY=${CMAKE_BINARY_DIR}/libnettle/install/lib/libnettle.a")
LIST(APPEND PRJ_CMAKE_ARGS "-DGMP_LIBRARY=${CMAKE_BINARY_DIR}/gmp/install/lib/libgmp.a")
LIST(APPEND PRJ_CMAKE_ARGS "-DGNUTLS_INCLUDE_DIR=${CMAKE_BINARY_DIR}/gnutls/install/include")
LIST(APPEND PRJ_CMAKE_ARGS "-DCMAKE_TOOLCHAIN_FILE=${CMAKE_TOOLCHAIN_FILE}")
LIST(APPEND PRJ_CMAKE_ARGS "-DCMAKE_INSTALL_PREFIX=''")

ExternalProject_Add(TARGET_mariadbconnector
                    PREFIX ${CMAKE_CURRENT_BINARY_DIR}/mariadbconnector
                    URL ${CMAKE_CURRENT_SOURCE_DIR}/mariadb-connector-c-3.3.5-src.tar.gz
                    URL_HASH SHA1=ab5d1da4ec547e23349b80adec5f1a225d3b3cb1
                    PATCH_COMMAND "${PYTHON_EXECUTABLE}" ${CMAKE_HOME_DIRECTORY}/cmake/tools/apply_patches.py --working-folder ${CMAKE_CURRENT_BINARY_DIR}/mariadbconnector/src/TARGET_mariadbconnector --patch-folder ${CMAKE_CURRENT_SOURCE_DIR}/patches --strip 1
                    CMAKE_ARGS ${PRJ_CMAKE_ARGS}
                    INSTALL_COMMAND make install DESTDIR=${CMAKE_BINARY_DIR}/install
)
