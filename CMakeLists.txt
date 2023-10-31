project(libudevpp)

if( NOT DEFINED UDEVPP_LIBRARY_TYPE)
    message(WARNING ": UDEVPP_LIBRARY_TYPE not defined, using udevpp default lib type: static")
    set(UDEVPP_LIBRARY_TYPE STATIC)
endif()


add_library(${PROJECT_NAME} ${UDEVPP_LIBRARY_TYPE}
    src/device.cpp
    src/enumerate.cpp
    src/monitor.cpp
    src/udev.cpp
)

target_include_directories(${PROJECT_NAME} 
    PUBLIC ${PROJECT_SOURCE_DIR}/include 
    PUBLIC ${PROJECT_SOURCE_DIR}/interface
    PRIVATE ${PROJECT_SOURCE_DIR}/include/udev++ 
    PRIVATE ${PROJECT_SOURCE_DIR}/interface/udev++
)

add_library(${PROJECT_NAME}-interface INTERFACE)
target_include_directories(${PROJECT_NAME}-interface
    INTERFACE ${PROJECT_SOURCE_DIR}/interface
)


add_library(flo::udev++ ALIAS ${PROJECT_NAME})
add_library(flo::udev++-interface ALIAS ${PROJECT_NAME}-interface)


if ( UDEVPP_LIBRARY_TYPE STREQUAL SHARED)
    install(TARGETS ${PROJECT_NAME}
            OPTIONAL
            RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
            LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
            ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    )
endif()