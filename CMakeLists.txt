
cmake_minimum_required (VERSION 2.8)
if (POLICY CMP0048)
  cmake_policy(SET CMP0048 NEW)
endif (POLICY CMP0048)

project(cryptoTools VERSION 1.7.0)

project(libOTe)
set(libOTe_VERSION 1.3.0)


if("${CMAKE_CURRENT_SOURCE_DIR}" STREQUAL "${CMAKE_SOURCE_DIR}")

	############################################
	#          If top level cmake              #
	############################################
	if(${CMAKE_VERSION} VERSION_LESS "3.12.0") 
		message("Please consider updating CMake to 3.12+")
	endif()
	
	############################################
	#           Flag and #defines              #
	############################################
	add_definitions(-DSOLUTION_DIR='${CMAKE_SOURCE_DIR}')
	if(MSVC)


	else()
		set(COMMON_FLAGS "-Wall -march=native -Wfatal-errors")
		SET(CMAKE_CXX_FLAGS_RELEASE "-O3  -DNDEBUG") 
		SET(CMAKE_CXX_FLAGS_RELWITHDEBINFO " -O2 -g -ggdb")
		SET(CMAKE_CXX_FLAGS_DEBUG  "-O0 -g -ggdb")
		set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS}  -std=c++17")
	endif()

	set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${COMMON_FLAGS}")
	set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${COMMON_FLAGS}")
	
	# -Wno-ignored-attributes -Wno-parentheses
	# -maes -msse2 -msse3 -msse4.1 -mpclmul  
	# -std=c++14
	# -fPIC -no-pie 


	############################################
	#           Build mode checks              #
	############################################
	
	# Set a default build type for single-configuration
	# CMake generators if no build type is set.
	if(NOT CMAKE_CONFIGURATION_TYPES AND NOT CMAKE_BUILD_TYPE)
	   SET(CMAKE_BUILD_TYPE Release)
	endif()

	if(    NOT "${CMAKE_BUILD_TYPE}" STREQUAL "Release"
       AND NOT "${CMAKE_BUILD_TYPE}" STREQUAL "Debug" 
       AND NOT "${CMAKE_BUILD_TYPE}" STREQUAL "RelWithDebInfo" )
		  
        message(WARNING ": Unknown build type - \${CMAKE_BUILD_TYPE}=${CMAKE_BUILD_TYPE}.  Please use one of Debug, Release, or RelWithDebInfo. e.g. call\n\tcmake . -DCMAKE_BUILD_TYPE=Release\n" )        
	endif()
endif()
 
 
#############################################
#    Build cryptoTools (common utilities)   #
#############################################
add_subdirectory(cryptoTools)

 
#############################################
#                CONFIGURE                  #
#############################################

if(DEFINED ENABLE_ALL_OT)
	set(ENABLE_SIMPLESTOT     ${ENABLE_ALL_OT} CACHE BOOL "" FORCE)
	set(ENABLE_SIMPLESTOT_ASM ${ENABLE_ALL_OT} CACHE BOOL "" FORCE)
	set(ENABLE_MR             ${ENABLE_ALL_OT} CACHE BOOL "" FORCE)
	set(ENABLE_MR_KYBER       ${ENABLE_ALL_OT} CACHE BOOL "" FORCE)
	set(ENABLE_NP             ${ENABLE_ALL_OT} CACHE BOOL "" FORCE)
	set(ENABLE_KOS            ${ENABLE_ALL_OT} CACHE BOOL "" FORCE)
	set(ENABLE_IKNP           ${ENABLE_ALL_OT} CACHE BOOL "" FORCE)
	set(ENABLE_SILENTOT       ${ENABLE_ALL_OT} CACHE BOOL "" FORCE)
	set(ENABLE_DELTA_KOS      ${ENABLE_ALL_OT} CACHE BOOL "" FORCE)
	set(ENABLE_DELTA_IKNP     ${ENABLE_ALL_OT} CACHE BOOL "" FORCE)
	set(ENABLE_OOS            ${ENABLE_ALL_OT} CACHE BOOL "" FORCE)
	set(ENABLE_KKRT           ${ENABLE_ALL_OT} CACHE BOOL "" FORCE)
	set(ENABLE_RR             ${ENABLE_ALL_OT} CACHE BOOL "" FORCE)
	set(ENABLE_AKN            ${ENABLE_ALL_OT} CACHE BOOL "" FORCE)
	set(ENABLE_SILENT_VOLE    ${ENABLE_ALL_OT} CACHE BOOL "" FORCE)
	unset(ENABLE_ALL_OT CACHE)
endif()

option(ENABLE_SIMPLESTOT     "Build the SimplestOT base OT" OFF)
option(ENABLE_SIMPLESTOT_ASM "Build the assembly based SimplestOT library" OFF)
option(ENABLE_MR             "Build the MasnyRindal base OT" OFF)
option(ENABLE_MR_KYBER       "Build the Kyber (LWE based) library and MR-Kyber base OT" OFF)
option(ENABLE_NP             "Build the NaorPinkas base OT" OFF)

option(ENABLE_KOS            "Build the KOS OT-Ext protocol." OFF)
option(ENABLE_IKNP           "Build the IKNP OT-Ext protocol." OFF)
option(ENABLE_SILENTOT       "Build the Slient OT protocol." OFF)
option(ENABLE_DELTA_KOS      "Build the KOS Delta-OT-Ext protocol." OFF)
option(ENABLE_DELTA_IKNP     "Build the IKNP Delta-OT-Ext protocol." OFF)

option(ENABLE_OOS            "Build the OOS 1-oo-N OT-Ext protocol." OFF)
option(ENABLE_KKRT           "Build the KKRT 1-oo-N OT-Ext protocol." OFF)
option(ENABLE_RR             "Build the RR 1-oo-N OT-Ext protocol." OFF)

option(ENABLE_AKN            "Build the RR ~k-oo-N OT-Ext protocol." OFF)

option(ENABLE_SILENT_VOLE    "Build the Silent Vole protocol." OFF)


if(NOT UNIX OR APPLE OR MSVC)
	if(ENABLE_SIMPLESTOT_ASM)
		message("ENABLE_SIMPLESTOT_ASM only supported on Linux")
		set(ENABLE_SIMPLESTOT_ASM OFF CACHE BOOL "" FORCE)
	endif()
	if(ENABLE_MR_KYBER)
		message("ENABLE_MR_KYBER only supported on Linux")
		set(ENABLE_MR_KYBER OFF  CACHE BOOL "" FORCE)
	endif()

endif()

message(STATUS "General Options\n=======================================================")
message(STATUS "Option: ENABLE_ALL_OT         = ON/OFF\n\n")

message(STATUS "Base OT protocols\n=======================================================")
message(STATUS "Option: ENABLE_SIMPLESTOT     = ${ENABLE_SIMPLESTOT}")
message(STATUS "Option: ENABLE_SIMPLESTOT_ASM = ${ENABLE_SIMPLESTOT_ASM}")
message(STATUS "Option: ENABLE_MR             = ${ENABLE_MR}")
message(STATUS "Option: ENABLE_MR_KYBER       = ${ENABLE_MR_KYBER}")
message(STATUS "Option: ENABLE_NP             = ${ENABLE_NP}\n\n")

message(STATUS "1-out-of-2 OT Extension protocols\n=======================================================")
message(STATUS "Option: ENABLE_KOS            = ${ENABLE_KOS}")
message(STATUS "Option: ENABLE_IKNP           = ${ENABLE_IKNP}")
message(STATUS "Option: ENABLE_SILENTOT       = ${ENABLE_SILENTOT}\n\n")

message(STATUS "1-out-of-2 Delta-OT Extension protocols\n=======================================================")
message(STATUS "Option: ENABLE_DELTA_KOS      = ${ENABLE_DELTA_KOS}")
message(STATUS "Option: ENABLE_DELTA_IKNP     = ${ENABLE_DELTA_IKNP}\n\n")

message(STATUS "1-out-of-N OT Extension protocols\n=======================================================")
message(STATUS "Option: ENABLE_OOS            = ${ENABLE_OOS}")
message(STATUS "Option: ENABLE_KKRT           = ${ENABLE_KKRT}")
message(STATUS "Option: ENABLE_RR             = ${ENABLE_RR}\n\n")

message(STATUS "Vole protocols\n=======================================================")
message(STATUS "Option: ENABLE_SILENT_VOLE    = ${ENABLE_SILENT_VOLE}\n\n")


#############################################
#               Config Checks               #
#############################################

if( NOT ENABLE_ALL_OT AND
	NOT ENABLE_SIMPLESTOT AND 
	NOT ENABLE_SIMPLESTOT_ASM AND 
	NOT ENABLE_MR AND 
	NOT ENABLE_MR_KYBER AND 
	NOT ENABLE_NP) 
	message(WARNING "NO Base OT enabled.")
endif()

if ((ENABLE_ALL_OT OR ENABLE_MR) AND NOT (ENABLE_MIRACL OR ENABLE_RELIC))
   message(FATAL_ERROR "ENABLE_ALL_OT and ENABLE_MR requires ENABLE_MIRACL or ENABLE_RELIC")
endif()

if ((ENABLE_SIMPLESTOT OR ENABLE_MR OR ENABLE_NP) AND
   NOT (ENABLE_MIRACL OR ENABLE_RELIC))
   message(FATAL_ERROR "ENABLE_SIMPLESTOT and ENABLE_MR requires ENABLE_MIRACL or ENABLE_RELIC")
endif() 


file(REMOVE ${CMAKE_CURRENT_LIST_DIR}/libOTe/config.h)
configure_file(libOTe/config.h.in libOTe/config.h)


#############################################
#               Build libOTe                #
#############################################

if(ENABLE_SIMPLESTOT_ASM AND (NOT MSVC))
	add_subdirectory(SimplestOT)
endif()

if(ENABLE_MR_KYBER AND (NOT MSVC))
	add_subdirectory(KyberOT)
endif()

add_subdirectory(libOTe)
add_subdirectory(libOTe_Tests)

#############################################
#              Build Frontend               #
#############################################
add_subdirectory(frontend)


