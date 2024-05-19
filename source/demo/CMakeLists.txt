# -------------------------------
# Macro that will define our source groups
# -------------------------------
MACRO(GROUPSOURCES curdir folder_name)
    FILE(GLOB children RELATIVE ${curdir} ${curdir}/*)
    FOREACH(child ${children})
           IF(IS_DIRECTORY ${curdir}/${child})
              GROUPSOURCES(${curdir}/${child} ${folder_name}/${child})
           ELSE()
              SOURCE_GROUP(${folder_name} FILES ${curdir}/${child})
           ENDIF()
    ENDFOREACH()
ENDMACRO(GROUPSOURCES)

# -------------------------------
# Setup exe
# -------------------------------
add_executable(${PROJECT_NAME_LOWER}_app)

if(MSVC)
    add_definitions(-D${PROJECT_NAME_UPPER}_COMPILER_MSVC)
endif()

# Sources
## private sources
target_sources(${PROJECT_NAME_LOWER}_app PRIVATE 
    ### fmt
    ${CMAKE_CURRENT_LIST_DIR}/private/fmt/core.h
    ${CMAKE_CURRENT_LIST_DIR}/private/fmt/format-inl.h
    ${CMAKE_CURRENT_LIST_DIR}/private/fmt/format.h

    ${CMAKE_CURRENT_LIST_DIR}/private/util/log.h
    ${CMAKE_CURRENT_LIST_DIR}/private/util/types.h
    ${CMAKE_CURRENT_LIST_DIR}/private/main.cpp)

# Create the project filters
GROUPSOURCES(${CMAKE_CURRENT_LIST_DIR}/private private)

# Private includes
target_include_directories(${PROJECT_NAME_LOWER}_app PRIVATE ${CMAKE_CURRENT_LIST_DIR}/private)

# Compile definitions
if(WIN32)
    target_compile_definitions(${PROJECT_NAME_LOWER}_app PRIVATE ${PROJECT_NAME_UPPER}_PLATFORM_WINDOWS)
endif()
                      