cmake_minimum_required(VERSION 3.16)

project(qcnpuperfd
	VERSION 1.0.0
	DESCRIPTION "Qualcomm NPU performance stats daemon"
	LANGUAGES C
)

include(GNUInstallDirs)
find_package(PkgConfig REQUIRED)
pkg_check_modules(QCNPUPERF REQUIRED qcnpuperf)

# qcom_dsp.h includes "remote.h" which lives in the fastrpc include dir.
# libfastrpc-dev has no .pc file, so locate the header explicitly.
find_path(FASTRPC_INCLUDE_DIR remote.h PATH_SUFFIXES fastrpc)

add_executable(qcnpuperfd src/qcnpuperfd.c)

target_include_directories(qcnpuperfd PRIVATE
    ${QCNPUPERF_INCLUDE_DIRS}
    ${FASTRPC_INCLUDE_DIR}
)
target_link_directories(qcnpuperfd PRIVATE ${QCNPUPERF_LIBRARY_DIRS})
target_link_libraries(qcnpuperfd PRIVATE ${QCNPUPERF_LIBRARIES})

install(TARGETS qcnpuperfd RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR})
