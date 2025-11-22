#
# App
#

set(SOURCES
    Source/Main.cpp
    Source/AppLayer.h
    Source/AppLayer.cpp
    Source/VoidLayer.h
    Source/VoidLayer.cpp
    Source/OverlayLayer.h
    Source/OverlayLayer.cpp
)

add_executable(App)

target_sources(App PRIVATE ${SOURCES})

# Match VS filters to directory structure on disk
source_group(TREE "${CMAKE_CURRENT_SOURCE_DIR}" FILES ${SOURCES})

add_compile_definitions(
    GLFW_INCLUDE_NONE
)

target_link_libraries(App Core)

target_include_directories(App PRIVATE Source)

# Set working directory for VS debugger
if (WIN32)
    set_target_properties(App PROPERTIES
        VS_DEBUGGER_WORKING_DIRECTORY "${CMAKE_SOURCE_DIR}/App"
    )
endif()


# Copy over Shaders / Other assets for the non VS plebs
if (UNIX)
    add_custom_target(copy_shaders
        COMMAND ${CMAKE_COMMAND} -E copy_directory "${CMAKE_SOURCE_DIR}/App/Shaders" "${CMAKE_CURRENT_BINARY_DIR}/Shaders"
    )
    add_dependencies(App copy_shaders)
endif()
