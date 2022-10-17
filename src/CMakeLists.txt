# The MIT License (MIT)
#
# Copyright (c) 2022 Mikhail Svetkin
#
# Permission is hereby granted, free of charge, to any person obtaining a copy
# of this software and associated documentation files (the "Software"), to deal
# in the Software without restriction, including without limitation the rights
# to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
# copies of the Software, and to permit persons to whom the Software is
# furnished to do so, subject to the following conditions:
#
# The above copyright notice and this permission notice shall be included in all
# copies or substantial portions of the Software.
#
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
# IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
# FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
# AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
# LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
# OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
# SOFTWARE.

cmake_minimum_required(VERSION 3.21)

project(some-qt-lib VERSION 0.0.1 LANGUAGES CXX)

list(APPEND CMAKE_MODULE_PATH "${PROJECT_SOURCE_DIR}/cmake")

include(GNUInstallDirs)

set(PROJECT_PREFIX some-qt-lib)
include(SomeQtLibModule)

add_subdirectory(core)

# project-wide wrapper
add_library(some-qt-lib INTERFACE)
target_link_libraries(some-qt-lib INTERFACE some-qt-lib::core)
add_library(some-qt-lib::some-qt-lib ALIAS some-qt-lib)
install(TARGETS some-qt-lib EXPORT ${TARGETS_EXPORT_NAME})

include(CMakePackageConfigHelpers)
write_basic_package_version_file(
  ${CMAKE_CURRENT_BINARY_DIR}/some-qt-lib-config-version.cmake
  COMPATIBILITY SameMajorVersion)
configure_package_config_file(some-qt-lib-config.cmake.in
  ${CMAKE_CURRENT_BINARY_DIR}/some-qt-lib-config.cmake
  INSTALL_DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/some-qt-lib)

# installation
install(
  EXPORT ${TARGETS_EXPORT_NAME}
  DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/some-qt-lib
  NAMESPACE some-qt-lib::)

install(
  FILES
    ${CMAKE_CURRENT_BINARY_DIR}/some-qt-lib-config.cmake
    ${CMAKE_CURRENT_BINARY_DIR}/some-qt-lib-config-version.cmake
  DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/some-qt-lib
)
