#
# $Id$
#
# (C) Copyright 2011 Johns Hopkins University (JHU), All Rights Reserved.
#
# --- begin cisst license - do not edit ---
#
# This software is provided "as is" under an open source license, with
# no warranty.  The complete license can be found in license.txt and
# http://www.cisst.org/cisst/license.txt.
#
# --- end cisst license ---

set (REQUIRED_CISST_LIBRARIES
     cisstCommon
     cisstVector
     cisstNumerical
     cisstOSAbstraction
     cisstRobot
     cisstMultiTask
     cisstParameterTypes
     )

find_package(cisst REQUIRED ${REQUIRED_CISST_LIBRARIES})

find_package(OpenGL REQUIRED)
find_package(GLUT REQUIRED)

if(cisst_FOUND_AS_REQUIRED AND OPENGL_FOUND AND GLUT_FOUND)

  # load cisst configuration
  include (${CISST_USE_FILE})

  set(HEADER_FILES 
    osaGeometry.h
    osaMeshTriangular.h
    ../osaGLUT.h
    ../osaGLUTManipulator.h
    ../mtsGLUTManipulator.h
    ../sawGLUTSimulatorExport.h
    )
    
  set(SOURCE_FILES
    osaGeometry.cpp
    osaMeshTriangular.cpp
    osaGLUT.cpp
    osaGLUTManipulator.cpp
    mtsGLUTManipulator.cpp
    )

  # Following isn't really needed
  # include_directories(${OPENGL_INCLUDE_DIR} ${GLUT_INCLUDE_DIR})

  include_directories(${CMAKE_CURRENT_SOURCE_DIR})

  add_library(sawGLUTSimulator ${HEADER_FILES} ${SOURCE_FILES})

  target_link_libraries(sawGLUTSimulator ${OPENGL_LIBRARIES} ${GLUT_LIBRARIES})

  cisst_target_link_libraries(sawGLUTSimulator ${REQUIRED_CISST_LIBRARIES})

  # create/configure file for find_package (sawGLUTSimulator)
  set (sawGLUTSimulator_INCLUDE_DIR "${sawComponents_SOURCE_DIR}" "${sawComponents_BINARY_DIR}")
  set (sawGLUTSimulator_LIBRARY_DIR "${LIBRARY_OUTPUT_PATH}")
  set (sawGLUTSimulator_LIBRARIES sawGLUTSimulator)

  configure_file ("${CMAKE_CURRENT_SOURCE_DIR}/sawGLUTSimulatorConfig.cmake.in"
                  "${sawComponents_BINARY_DIR}/sawGLUTSimulatorConfig.cmake"
                   @ONLY@)

else(cisst_FOUND_AS_REQUIRED AND OPENGL_FOUND AND GLUT_FOUND)
  message(WARNING "sawGLUTSimulator will not be built (missing dependencies)")
endif(cisst_FOUND_AS_REQUIRED AND OPENGL_FOUND AND GLUT_FOUND)
