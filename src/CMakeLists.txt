###########################################################################
#   Copyright (C) 1998-2019 by authors (see AUTHORS.txt)                  #
#                                                                         #
#   This file is part of LuxMark.                                         #
#                                                                         #
#   LuxMark is free software; you can redistribute it and/or modify       #
#   it under the terms of the GNU General Public License as published by  #
#   the Free Software Foundation; either version 3 of the License, or     #
#   (at your option) any later version.                                   #
#                                                                         #
#   LuxMark is distributed in the hope that it will be useful,            #
#   but WITHOUT ANY WARRANTY; without even the implied warranty of        #
#   MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the         #
#   GNU General Public License for more details.                          #
#                                                                         #
#   You should have received a copy of the GNU General Public License     #
#   along with this program.  If not, see <http://www.gnu.org/licenses/>. #
#                                                                         #
#   LuxMark website: https://www.luxcorerender.org                        #
###########################################################################

set(LUXMARK_VERSION_MAJOR 4)
set(LUXMARK_VERSION_MINOR 0alpha0)

# Instruct CMake to run moc automatically when needed
set(CMAKE_AUTOMOC ON)
# Create code from a list of Qt designer ui files
set(CMAKE_AUTOUIC ON)

# Configure a header file to pass some of the CMake settings
# to the source code
configure_file(
  "${CMAKE_CURRENT_SOURCE_DIR}/luxmarkcfg.h.in"
  "${CMAKE_CURRENT_SOURCE_DIR}/luxmarkcfg.h"
  )

#############################################################################
#
# LuxMark binary
#
#############################################################################

set(LUXMARK_SRCS
	aboutdialog.cpp
    hardwaretree.cpp
	luxmarkapp.cpp
	main.cpp
	mainwindow.cpp
	luxcoreuidialog.cpp
    resultdialog.cpp
	submitdialog.cpp
	luxcorerendersession.cpp
	convtest/convtest.cpp
	convtest/pdiff/lpyramid.cpp
	convtest/pdiff/metric.cpp
	)
set(LUXMARK_MOC
	aboutdialog.h
	mainwindow.h
    hardwaretree.h
	luxmarkapp.h
	luxcoreuidialog.h
    resultdialog.h
	submitdialog.h
	)
set(LUXMARK_UIS
	aboutdialog.ui
	mainwindow.ui
	luxcoreuidialog.ui
    resultdialog.ui
	submitdialog.ui
	)
set(LUXMARK_RCS
	resources.qrc
	)

QT5_ADD_RESOURCES(LUXMARK_RC_SRCS ${LUXMARK_RCS})
QT5_WRAP_UI(LUXMARK_UI_HDRS ${LUXMARK_UIS})
QT5_WRAP_CPP(LUXMARK_MOC_SRCS ${LUXMARK_MOC})

include_directories(".")
INCLUDE_DIRECTORIES(${CMAKE_CURRENT_BINARY_DIR})

set(LUXMARK_SRCS
	${LUXMARK_SRCS}
	${LUXMARK_UI_HDRS}
	${LUXMARK_RC_SRCS}
	${LUXMARK_MOC_SRCS}
	)

#############################################################################

ADD_EXECUTABLE(luxmark WIN32 ${LUXMARK_SRCS})

TARGET_LINK_LIBRARIES(luxmark ${ALL_LUXCORE_LIBRARIES} ${Boost_LIBRARIES} ${Qt5_LIBRARIES} ${OPENGL_gl_LIBRARY} ${OPENCL_LIBRARIES})

if (WIN32)
	# This is needed by Boost 1.67 but is not found automatically
    TARGET_LINK_LIBRARIES(luxmark bcrypt.lib)
endif(WIN32)

if(WIN32)
	set_target_properties(luxmark PROPERTIES LINK_FLAGS_DEBUG "/SUBSYSTEM:CONSOLE")
	set_target_properties(luxmark PROPERTIES COMPILE_DEFINITIONS_DEBUG "_CONSOLE")
	set_target_properties(luxmark PROPERTIES LINK_FLAGS_RELWITHDEBINFO "/SUBSYSTEM:CONSOLE")
	set_target_properties(luxmark PROPERTIES COMPILE_DEFINITIONS_RELWITHDEBINFO "_CONSOLE")
	#set_target_properties(luxmark PROPERTIES LINK_FLAGS_RELEASE "/SUBSYSTEM:WINDOWS /ENTRY:mainCRTStartup")
	#set_target_properties(luxmark PROPERTIES LINK_FLAGS_MINSIZEREL "/SUBSYSTEM:WINDOWS /ENTRY:mainCRTStartup")
endif(WIN32)
