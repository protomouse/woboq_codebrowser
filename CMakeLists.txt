cmake_minimum_required(VERSION 2.8)

set(CPACK_PACKAGE_NAME woboq-codebrowser)
set(CPACK_PACKAGE_VENDOR Woboq)
set(CPACK_PACKAGE_VERSION 1.6.0)
set(CPACK_SET_DESTDIR TRUE)
set(CPACK_STRIP_FILES TRUE)

set(CPACK_DEBIAN_PACKAGE_VERSION ${CPACK_PACKAGE_VERSION}-1)
set(CPACK_DEBIAN_PACKAGE_MAINTAINER "protomouse <root@protomou.se>")
set(CPACK_DEBIAN_PACKAGE_DESCRIPTION "web-based code browser (static site generator)
 The Woboq Code Browser is a web-based code browser for your C/C++ projects.")
set(CPACK_DEBIAN_PACKAGE_SECTION misc)
set(CPACK_DEBIAN_PACKAGE_PRIORITY optional)
set(CPACK_DEBIAN_PACKAGE_DEPENDS "zlib1g, libc6, libffi5, libstdc++6, libgcc1")

if(CPACK_GENERATOR STREQUAL "DEB")
  set(CPACK_PACKAGE_FILE_NAME "${CPACK_PACKAGE_NAME}_${CPACK_DEBIAN_PACKAGE_VERSION}_${CPACK_DEBIAN_PACKAGE_ARCHITECTURE}")
  set(CMAKE_INSTALL_PREFIX /usr)

  set(ENV{DEB_CXXFLAGS_STRIP} "-g -O2")
  exec_program(dpkg-buildflags ARGS --get CXXFLAGS OUTPUT_VARIABLE DPKG_CXXFLAGS)
  MESSAGE(STATUS "dpkg CXXFLAGS: ${DPKG_CXXFLAGS}")
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${DPKG_CXXFLAGS}")

  exec_program(dpkg-buildflags ARGS --get LDFLAGS OUTPUT_VARIABLE DPKG_LDFLAGS)
  MESSAGE(STATUS "dpkg LDFLAGS: ${DPKG_LDFLAGS}")
  set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} ${DPKG_LDFLAGS}")
endif()

add_subdirectory(generator)
add_subdirectory(indexgenerator)

set(CPACK_INSTALL_CMAKE_PROJECTS
  ${codebrowser_generator_BINARY_DIR} codebrowser_generator ALL /
  ${codebrowser_indexgenerator_BINARY_DIR} codebrowser_indexgenerator ALL /
)

include(CPack)