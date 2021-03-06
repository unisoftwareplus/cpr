message(STATUS "Using CURL_INCLUDE_DIRS: ${CURL_INCLUDE_DIRS}.")

add_library(${CPR_LIBRARIES} SHARED

    # Source files
    auth.cpp
    cookies.cpp
    cprtypes.cpp
    digest.cpp
    error.cpp
    multipart.cpp
    ntlm.cpp
    parameters.cpp
    payload.cpp
    proxies.cpp
    session.cpp
    timeout.cpp
    util.cpp
    unix_socket.cpp
    curlholder.cpp

    # Header files (useful in IDEs)
    "${CPR_INCLUDE_DIRS}/cpr/api.h"
    "${CPR_INCLUDE_DIRS}/cpr/auth.h"
    "${CPR_INCLUDE_DIRS}/cpr/body.h"
    "${CPR_INCLUDE_DIRS}/cpr/cookies.h"
    "${CPR_INCLUDE_DIRS}/cpr/cpr.h"
    "${CPR_INCLUDE_DIRS}/cpr/cprtypes.h"
    "${CPR_INCLUDE_DIRS}/cpr/curlholder.h"
    "${CPR_INCLUDE_DIRS}/cpr/defines.h"
    "${CPR_INCLUDE_DIRS}/cpr/digest.h"
    "${CPR_INCLUDE_DIRS}/cpr/error.h"
    "${CPR_INCLUDE_DIRS}/cpr/max_redirects.h"
    "${CPR_INCLUDE_DIRS}/cpr/multipart.h"
    "${CPR_INCLUDE_DIRS}/cpr/ntlm.h"
    "${CPR_INCLUDE_DIRS}/cpr/parameters.h"
    "${CPR_INCLUDE_DIRS}/cpr/payload.h"
    "${CPR_INCLUDE_DIRS}/cpr/proxies.h"
    "${CPR_INCLUDE_DIRS}/cpr/response.h"
    "${CPR_INCLUDE_DIRS}/cpr/session.h"
    "${CPR_INCLUDE_DIRS}/cpr/timeout.h"
    "${CPR_INCLUDE_DIRS}/cpr/util.h"
    "${CPR_INCLUDE_DIRS}/cpr/ssl_options.h"
    "${CPR_INCLUDE_DIRS}/cpr/limit_rate.h"
    "${CPR_INCLUDE_DIRS}/cpr/verbose.h"
    "${CPR_INCLUDE_DIRS}/cpr/unix_socket.h")

message(STATUS "Using CURL_LIBRARIES: ${CURL_LIBRARIES}.")
target_link_libraries(${CPR_LIBRARIES}
    ${CURL_LIBRARIES})

if(NOT (CMAKE_VERSION VERSION_LESS 3.0))
    target_include_directories(${CPR_LIBRARIES}
        PUBLIC
        PRIVATE ${CPR_INCLUDE_DIRS}
        PUBLIC ${CURL_INCLUDE_DIRS}
        INTERFACE $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>)
else()
    include_directories(
        ${CPR_INCLUDE_DIRS}
        ${CURL_INCLUDE_DIRS})
endif()

# Set version for shared libraries.
set_target_properties(${CPR_LIBRARIES}
     PROPERTIES
     VERSION ${${PROJECT_NAME}_VERSION}
     SOVERSION ${${PROJECT_NAME}_VERSION_MAJOR})

install(TARGETS ${CPR_LIBRARIES}
        EXPORT cpr_targets
        RUNTIME DESTINATION bin
        LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
        ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR})

install(EXPORT cpr_targets
        FILE cpr_targets.cmake
        DESTINATION ${ConfigPackageLocation})