cmake_minimum_required(VERSION 3.20)

project(MyPlugin VERSION 1.0.0.0)

find_package(CommonLibSSE REQUIRED)

configure_file(cmake/version.h.in version.h)
configure_file(cmake/version.rc.in version.rc)

add_library("${PROJECT_NAME}" SHARED)

target_sources("${PROJECT_NAME}"
	PRIVATE
	src/main.cpp
	src/pch.h
	"${PROJECT_BINARY_DIR}/version.h"
	"${PROJECT_BINARY_DIR}/version.rc"
)

target_precompile_headers("${PROJECT_NAME}"
	PRIVATE
	src/pch.h
)

target_compile_features("${PROJECT_NAME}"
	PRIVATE
	cxx_std_20
)

target_include_directories("${PROJECT_NAME}"
	PRIVATE
	"${PROJECT_BINARY_DIR}"
)

target_link_libraries("${PROJECT_NAME}"
	PRIVATE
	CommonLibSSE::CommonLibSSE
)

install(TARGETS "${PROJECT_NAME}" RUNTIME DESTINATION "${SKYRIM_ROOT}/data/skse/plugins")