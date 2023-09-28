cmake_minimum_required(VERSION 3.25)
project(nwc-toolkit CXX)

find_package(ICU REQUIRED i18n uc data)
find_package(Iconv REQUIRED)
find_package(ZLIB REQUIRED)
find_package(BZip2 REQUIRED)
find_package(LibLZMA REQUIRED)

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include ${ICU_INCLUDE_DIRS} ${Iconv_INCLUDE_DIRS} ${ZLIB_INCLUDE_DIRS} ${BZIP2_INCLUDE_DIRS} ${LIBLZMA_INCLUDE_DIRS})

set(libs nwc-toolkit lzma bz2 z ICU::i18n ICU::uc ICU::data)

# libnwc-toolkit
set(nwc_lib_dir lib/)
file(GLOB lib_src ${nwc_lib_dir}/*.cc)
add_library(nwc-toolkit STATIC ${lib_src})

# tools
set(nwc_tools_dir tools/)
file(GLOB tools_src ${nwc_tools_dir}/*.cc)
foreach(src ${tools_src})
  string(REPLACE "${CMAKE_CURRENT_SOURCE_DIR}/${nwc_tools_dir}/" "" temp ${src})
  string(REPLACE ".cc" "" commandname ${temp})
  add_executable(${commandname} ${src})
  target_link_libraries(${commandname} ${libs})
endforeach()

# tests
set(nwc_tests_dir tests/)
file(GLOB tests_src ${nwc_tests_dir}/*.cc)
foreach(src ${tests_src})
  string(REPLACE "${CMAKE_CURRENT_SOURCE_DIR}/${nwc_tests_dir}/" "" temp ${src})
  string(REPLACE ".cc" "" commandname ${temp})
  add_executable(${commandname} ${src})
  target_link_libraries(${commandname} ${libs})
endforeach()
