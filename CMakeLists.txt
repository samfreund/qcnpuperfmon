cmake_minimum_required(VERSION 3.16)

project(qcnpuperfd
	VERSION 1.0.0
	DESCRIPTION "Qualcomm NPU performance stats daemon"
	LANGUAGES C
)

include(GNUInstallDirs)
find_package(PkgConfig REQUIRED)
pkg_check_modules(QCNPUPERF REQUIRED qcnpuperf)

add_executable(qcnpuperfd src/qcnpuperfd.c)

target_include_directories(qcnpuperfd PRIVATE ${QCNPUPERF_INCLUDE_DIRS})
target_link_directories(qcnpuperfd PRIVATE ${QCNPUPERF_LIBRARY_DIRS})
target_link_libraries(qcnpuperfd PRIVATE ${QCNPUPERF_LIBRARIES})

install(TARGETS qcnpuperfd RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR})
install(FILES qcnpuperfd.service DESTINATION lib/systemd/system)
