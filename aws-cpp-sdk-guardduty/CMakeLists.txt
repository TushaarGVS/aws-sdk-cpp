add_project(aws-cpp-sdk-guardduty "C++ SDK for the AWS guardduty service" aws-cpp-sdk-core)

file(GLOB AWS_GUARDDUTY_HEADERS
    "include/aws/guardduty/*.h"
)

file(GLOB AWS_GUARDDUTY_MODEL_HEADERS
    "include/aws/guardduty/model/*.h"
)

file(GLOB AWS_GUARDDUTY_SOURCE
    "source/*.cpp"
)

file(GLOB AWS_GUARDDUTY_MODEL_SOURCE
    "source/model/*.cpp"
)

file(GLOB GUARDDUTY_UNIFIED_HEADERS
    ${AWS_GUARDDUTY_HEADERS}
    ${AWS_GUARDDUTY_MODEL_HEADERS}
)

file(GLOB GUARDDUTY_UNITY_SRC
    ${AWS_GUARDDUTY_SOURCE}
    ${AWS_GUARDDUTY_MODEL_SOURCE}
)

if(ENABLE_UNITY_BUILD)
    enable_unity_build("GUARDDUTY" GUARDDUTY_UNITY_SRC)
endif()

file(GLOB GUARDDUTY_SRC
    ${GUARDDUTY_UNIFIED_HEADERS}
    ${GUARDDUTY_UNITY_SRC}
)

if(WIN32)
    #if we are compiling for visual studio, create a sane directory tree.
    if(MSVC)
        source_group("Header Files\\aws\\guardduty" FILES ${AWS_GUARDDUTY_HEADERS})
        source_group("Header Files\\aws\\guardduty\\model" FILES ${AWS_GUARDDUTY_MODEL_HEADERS})
        source_group("Source Files" FILES ${AWS_GUARDDUTY_SOURCE})
        source_group("Source Files\\model" FILES ${AWS_GUARDDUTY_MODEL_SOURCE})
    endif(MSVC)
endif()

set(GUARDDUTY_INCLUDES
    "${CMAKE_CURRENT_SOURCE_DIR}/include/"
)

add_library(${PROJECT_NAME} ${LIBTYPE} ${GUARDDUTY_SRC})
add_library(AWS::${PROJECT_NAME} ALIAS ${PROJECT_NAME})

set_compiler_flags(${PROJECT_NAME})
set_compiler_warnings(${PROJECT_NAME})

if(USE_WINDOWS_DLL_SEMANTICS AND BUILD_SHARED_LIBS)
    target_compile_definitions(${PROJECT_NAME} PRIVATE "AWS_GUARDDUTY_EXPORTS")
endif()

target_include_directories(${PROJECT_NAME} PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>)

target_link_libraries(${PROJECT_NAME} PRIVATE ${PLATFORM_DEP_LIBS} ${PROJECT_LIBS})


setup_install()

install (FILES ${AWS_GUARDDUTY_HEADERS} DESTINATION ${INCLUDE_DIRECTORY}/aws/guardduty)
install (FILES ${AWS_GUARDDUTY_MODEL_HEADERS} DESTINATION ${INCLUDE_DIRECTORY}/aws/guardduty/model)

if(PLATFORM_WINDOWS AND MSVC)
    install (FILES nuget/${PROJECT_NAME}.autopkg DESTINATION nuget)
endif()

do_packaging()


