
set(UT_SRC
  unit1300.c
  unit1301.c
  unit1302.c
  unit1303.c
  unit1304.c
  unit1305.c
  unit1307.c
  unit1308.c
  unit1309.c
  unit1330.c
# Broken link on Linux
#  unit1394.c
  unit1395.c
  unit1396.c
  unit1397.c
  unit1398.c
  unit1600.c
  unit1601.c
  unit1603.c
# Broken link on Linux
#  unit1604.c
  )

set(UT_COMMON_FILES ../libtest/first.c ../libtest/test.h curlcheck.h)
include_directories(
  ${CURL_SOURCE_DIR}/lib          # To be able to reach "curl_setup_once.h"
  ${CURL_SOURCE_DIR}/tests/libtest
  ${CURL_SOURCE_DIR}/src
  ${CURL_BINARY_DIR}/lib          # To be able to reach "curl_config.h"
  ${CURL_BINARY_DIR}/include      # To be able to reach "curl/curlbuild.h"
)

foreach(_testfile ${UT_SRC})

  get_filename_component(_testname ${_testfile} NAME_WE)
  add_executable(${_testname} ${_testfile} ${UT_COMMON_FILES})
  target_link_libraries(${_testname} libcurl ${CURL_LIBS})
  set_target_properties(${_testname}
      PROPERTIES COMPILE_DEFINITIONS "UNITTESTS")

  add_test(NAME ${_testname}
           COMMAND ${_testname} "http://www.google.com"
           )
endforeach()
