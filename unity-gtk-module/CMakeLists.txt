cmake_minimum_required (VERSION 2.8)
cmake_policy (VERSION 2.8)
project (appmenu-gtk-module C)
enable_testing ()
include(GNUInstallDirs)
include(CMakeDependentOption)
find_package (PkgConfig QUIET)
pkg_check_modules(GTK2
	gtk+-2.0>=2.24)
pkg_check_modules(GTK3 REQUIRED QUIET
	gtk+-3.0>=3.10)
find_package(X11)

cmake_dependent_option(STANDALONE "Build unity-gtk-module STANDALONE" ON
	"NOT ENABLE_UNITY_GTK_MODULE;NOT CORE_FOUND;X11_FOUND" OFF)
cmake_dependent_option(ENABLE_UNITY_GTK_MODULE "Build unity-gtk-module" OFF
	"X11_FOUND" OFF)
cmake_dependent_option(ENABLE_GTK_MODULE_DEMOS "Enable demo programs and docs for gtk module" OFF
	"ENABLE_UNITY_GTK_MODULE" OFF)

if(STANDALONE)
	set (CPACK_SOURCE_PACKAGE_FILE_NAME "${CMAKE_PROJECT_NAME}")
	set (CPACK_SOURCE_IGNORE_FILES "/build/;/.gitmodules;/.git;/.gitignore;~$;${CPACK_SOURCE_IGNORE_FILES}")
	include (CPack)
	add_custom_target (dist COMMAND ${CMAKE_MAKE_PROGRAM} package_source)
endif()

if(ENABLE_GTK_MODULE_DEMOS)
	find_package(GtkDoc)
	if(GTKDOC_FOUND)
#		add_subdirectory(docs)
	endif()
endif()
if(ENABLE_UNITY_GTK_MODULE OR STANDALONE)
	add_subdirectory(data)
	add_subdirectory(lib)
	add_subdirectory(src)
	add_subdirectory(tests)
endif()
