# Copyright 2019 Temco Controls 

# Permission is hereby granted, free of charge, to any person obtaining a copy
# of this software and associated documentation files (the "Software"), to deal
# in the Software without restriction, including without limitation the 
# rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell 
# copies of the Software, and to permit persons to whom the Software is furnished to do so, 
# subject to the following conditions:

# The above copyright notice and this permission notice shall be included
# in all copies or substantial portions of the Software.

# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, 
# EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
# FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS 
# OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER 
# IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR 
# IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

# We support CMake starting at version 3.0.0
cmake_minimum_required(VERSION 3.0.0 FATAL_ERROR)

################### Variables. ####################
# Change if you want modify path or other values. #
###################################################

set(PROJECT_NAME T3000)

# Output Variables
set(OUTPUT_DEBUG Build/${CMAKE_BUILD_TYPE})
set(OUTPUT_REL Build/${CMAKE_BUILD_TYPE})

# Folders files
set(ISP_SOURCE_DIR ../ISP)
set(BADO_SOURCE_DIR ./BADO)
set(CHARTCTRL_SOURCE_DIR ../Modbus\ Poll/ChartCtrl)
set(AIRQUALITY_SOURCE_DIR ./AirQuality)
set(BACNET_SOURCE_DIR ./)
set(BACNET_SOURCE_CM5_DIR ./CM5)
set(CLASSCTRL_SOURCE_DIR ./Class)
set(CHARTCTRL_SOURCE_DIR ./ChartCtrl)
set(SINGLESLIDER_SOURCE_DIR ./singleslider)
set(SQLITE_DRIVER_DIR ../SQLiteDriver)
set(T3_SOURCE_DIR ./T3)
set(LIGHTINGCONTROLLER_SRC_DIR ./LightingController)
set(LIGHTINGCONTROLLER_HDR_DIR ./LightingController)
set(T3000_SOURCE_DIR ./)
set(T3000_HEADER_DIR ./)
set(AIRQUALITY_HEADER_DIR ./AirQuality)
set(BADO_HEADER_DIR ./BADO)
set(BACNET_HEADER_DIR ./)
set(CLASSCTRL_HEADER_DIR ./Class)
set(SINGLESLIDER_HEADER_DIR ./singleslider)
set(SQLITE_HEADER_DIR ../SQLiteDriver)
set(T3_HEADER_DIR ./T3)
set(HEADER_DIR_CM5 ./CM5)
set(HEADER_DIR_SQLITE SqliteLib)
set(HEADER_DIR_LIGHTINGCTRL ./LightingController)

set(HEADER_DIR_10 ADO)


############## CMake Project ################
#        The main options of project        #
#############################################

project(${PROJECT_NAME} CXX)

# Define Release by default.
if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE "Release")
  message(STATUS "Build type not specified: Use Release by default.")
endif(NOT CMAKE_BUILD_TYPE)

# Definition of Macros
add_definitions(
   -D_DEBUG 
   -DUNICODE
   -D_UNICODE
)

if(CMAKE_BUILD_TYPE STREQUAL "Debug")
  add_definitions(-D_DEBUG)
else()
  add_definitions(-D_NDEBUG)
endif()

############## Artefacts Output #################
# Defines outputs , depending Debug or Release. #
#################################################

if(CMAKE_BUILD_TYPE STREQUAL "Debug")
  set(CMAKE_LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/${OUTPUT_DEBUG}")
  set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/${OUTPUT_DEBUG}")
  set(CMAKE_EXECUTABLE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/${OUTPUT_DEBUG}")
else()
  set(CMAKE_LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/${OUTPUT_REL}")
  set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/${OUTPUT_REL}")
  set(CMAKE_EXECUTABLE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/${OUTPUT_REL}")
endif()

################# Flags ################
# Defines Flags for Windows and Linux. #
########################################

if(MSVC)
   set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} /W3 /EHsc")
   set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} /W3 /EHsc")
endif(MSVC)
if(NOT MSVC)
   set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
   if ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "Clang")
       set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -stdlib=libc++")
   endif()
endif(NOT MSVC)

################ Files ################
#   --   Add files to project.   --   #
#######################################

set(AIRQUALITY_FILES, 
  ${AIRQUALITY_SOURCE_DIR}/AirQuality.cpp
  ${AIRQUALITY_HEADER_DIR}/AirQuality.h
)

set(BACNET_FILES,
  ${BACNET_SOURCE_DIR}/Bacnetaddintodb.cpp
  ${BACNET_SOURCE_DIR}/BacnetAddLabel.cpp
  ${BACNET_SOURCE_DIR}/BacnetAddRemoteDevice.cpp
  ${BACNET_SOURCE_DIR}/BacnetAddVirtualDevice.cpp
  ${BACNET_SOURCE_DIR}/BacnetAlarmLog.cpp
  ${BACNET_SOURCE_DIR}/BacnetAlarmWindow.cpp
  ${BACNET_SOURCE_DIR}/BacnetAnalogCusRang.cpp
  ${BACNET_SOURCE_DIR}/BacnetAnnualRoutine.cpp
  ${BACNET_SOURCE_DIR}/BacnetATCommand.cpp
  ${BACNET_SOURCE_DIR}/BacnetController.cpp
  ${BACNET_SOURCE_DIR}/BacnetCustomerDigitalRange.cpp
  ${BACNET_SOURCE_DIR}/BacnetEditLabel.cpp
  ${BACNET_SOURCE_DIR}/BacnetGraphic.cpp
  ${BACNET_SOURCE_DIR}/BacnetGraphicSetting.cpp
  ${BACNET_SOURCE_DIR}/BacnetInput.cpp
  ${BACNET_SOURCE_DIR}/BacnetIOConfig.cpp
  ${BACNET_SOURCE_DIR}/BacnetMessageInput.cpp
  ${BACNET_SOURCE_DIR}/BacnetMonitor.cpp
  ${BACNET_SOURCE_DIR}/BacnetOutput.cpp
  ${BACNET_SOURCE_DIR}/BacnetProgram.cpp
  ${BACNET_SOURCE_DIR}/BacnetProgramDebug.cpp
  ${BACNET_SOURCE_DIR}/BacnetProgramEdit.cpp
  ${BACNET_SOURCE_DIR}/BacnetProgramSetting.cpp
  ${BACNET_SOURCE_DIR}/BacnetRange.cpp
  ${BACNET_SOURCE_DIR}/BacnetRegisterListView.cpp
  ${BACNET_SOURCE_DIR}/BacnetRemotePoint.cpp
  ${BACNET_SOURCE_DIR}/BacnetRemotePortWarning.cpp
  ${BACNET_SOURCE_DIR}/BacnetScheduleTime.cpp
  ${BACNET_SOURCE_DIR}/BacnetScreen.cpp
  ${BACNET_SOURCE_DIR}/BacnetScreenEdit.cpp
  ${BACNET_SOURCE_DIR}/BacnetSetting.cpp
  ${BACNET_SOURCE_DIR}/BacnetSettingHealth.cpp
  ${BACNET_SOURCE_DIR}/BacnetTool.cpp
  ${BACNET_SOURCE_DIR}/BacnetToolReadProperty.cpp
  ${BACNET_SOURCE_DIR}/BacnetTstat.cpp
  ${BACNET_SOURCE_DIR}/BacnetTstatSchedule.cpp
  ${BACNET_SOURCE_DIR}/BacnetUserConfig.cpp
  ${BACNET_SOURCE_DIR}/BacnetUserlogin.cpp
  ${BACNET_SOURCE_DIR}/BacnetVarCusRang.cpp
  ${BACNET_SOURCE_DIR}/BacnetVariable.cpp
  ${BACNET_SOURCE_DIR}/BacnetView.cpp
  ${BACNET_SOURCE_DIR}/BacnetWait.cpp
  ${BACNET_SOURCE_DIR}/BacnetWeeklyRoutine.cpp
  ${BACNET_SOURCE_DIR}/Bacnet_Range_Msv.cpp
  ${BACNET_SOURCE_CM5_DIR}/BacNetProgram_transplant.cpp
  ${BACNET_SOURCE_DIR}/ConnectRemoteServer.cpp
  ${BACNET_SOURCE_DIR}/DebugWindow.cpp
  ${BACNET_SOURCE_DIR}/Downloadfile.cpp
  ${BACNET_SOURCE_DIR}/DownloadSocket.cpp
  ${BACNET_SOURCE_DIR}/DuplicateIdDetected.cpp
  ${BACNET_SOURCE_DIR}/ISPModeSlove.cpp
  ${BACNET_SOURCE_DIR}/ListCtrlCellEx.cpp
  ${BACNET_SOURCE_DIR}/ListCtrlEx.cpp
  ${BACNET_SOURCE_DIR}/MyOwnListCtrl.cpp
  ${BACNET_SOURCE_DIR}/PointItem.cpp
  ${BACNET_SOURCE_DIR}/ShowMessageDlg.cpp
  ${BACNET_SOURCE_DIR}/SliderBoth.cpp
  ${BACNET_SOURCE_DIR}/SliderBothBtn.cpp
  ${BACNET_SOURCE_DIR}/Staticex.cpp
  ${BACNET_SOURCE_DIR}/StatusbarCtrl.cpp
  ${BACNET_SOURCE_DIR}/TCPServer.cpp
)

set(BACNET_CM5_H_FILES,
  ${BACNET_SOURCE_CM5_DIR}/DialogCM5.h
  ${BACNET_SOURCE_CM5_DIR}/ListCtrlCellEx.h
  ${BACNET_SOURCE_CM5_DIR}/ListCtrlEx.h
  ${BACNET_SOURCE_CM5_DIR}/mygdiplus.h
  ${BACNET_SOURCE_CM5_DIR}/MyOwnListCtrl.h
  ${BACNET_SOURCE_CM5_DIR}/PARSER.H
  ${BACNET_SOURCE_CM5_DIR}/PerfTimer.h
  ${BACNET_SOURCE_CM5_DIR}/ROUTER.H
  ${BACNET_SOURCE_CM5_DIR}/SliderBoth.h
  ${BACNET_SOURCE_CM5_DIR}/SliderBothBtn.h
  ${BACNET_SOURCE_CM5_DIR}/T3000DEF.H
  ${BACNET_SOURCE_CM5_DIR}/ud_str.h
  ${BACNET_SOURCE_CM5_DIR}/user_data.h
)

set(BADO_SOURCE_FILES,
  ${BADO_SOURCE_DIR}\BADO.cpp
  ${BADO_SOURCE_DIR}\BADO.h
)

set(CHARTCTRL_SOURCE_FILES,
  ${CHARTCTRL_SOURCE_DIR}/ChartAxis.cpp
  ${CHARTCTRL_SOURCE_DIR}/ChartAxis.h
  ${CHARTCTRL_SOURCE_DIR}/ChartAxisLabel.cpp
  ${CHARTCTRL_SOURCE_DIR}/ChartAxisLabel.h
  ${CHARTCTRL_SOURCE_DIR}/ChartBalloonLabel.h
  ${CHARTCTRL_SOURCE_DIR}/ChartBalloonLabel.inl
  ${CHARTCTRL_SOURCE_DIR}/ChartBarSerie.cpp
  ${CHARTCTRL_SOURCE_DIR}/ChartBarSerie.h
  ${CHARTCTRL_SOURCE_DIR}/ChartCandlestickSerie.cpp
  ${CHARTCTRL_SOURCE_DIR}/ChartCandlestickSerie.h
  ${CHARTCTRL_SOURCE_DIR}/ChartCrossHairCursor.cpp
  ${CHARTCTRL_SOURCE_DIR}/ChartCrossHairCursor.h
  ${CHARTCTRL_SOURCE_DIR}/ChartCtrl.cpp
  ${CHARTCTRL_SOURCE_DIR}/ChartCtrl.h
  ${CHARTCTRL_SOURCE_DIR}/ChartCursor.cpp
  ${CHARTCTRL_SOURCE_DIR}/ChartCursor.h
  ${CHARTCTRL_SOURCE_DIR}/ChartDateTimeAxis.cpp
  ${CHARTCTRL_SOURCE_DIR}/ChartDateTimeAxis.h
  ${CHARTCTRL_SOURCE_DIR}/ChartDragLineCursor.cpp
  ${CHARTCTRL_SOURCE_DIR}/ChartDragLineCursor.h
  ${CHARTCTRL_SOURCE_DIR}/ChartFont.cpp
  ${CHARTCTRL_SOURCE_DIR}/ChartFont.h
  ${CHARTCTRL_SOURCE_DIR}/ChartGanttSerie.cpp
  ${CHARTCTRL_SOURCE_DIR}/ChartGanttSerie.h
  ${CHARTCTRL_SOURCE_DIR}/ChartGradient.cpp
  ${CHARTCTRL_SOURCE_DIR}/ChartGradient.h
  ${CHARTCTRL_SOURCE_DIR}/ChartGrid.cpp
  ${CHARTCTRL_SOURCE_DIR}/ChartGrid.h
  ${CHARTCTRL_SOURCE_DIR}/ChartLabel.h
  ${CHARTCTRL_SOURCE_DIR}/ChartLabel.inl
  ${CHARTCTRL_SOURCE_DIR}/ChartLegend.cpp
  ${CHARTCTRL_SOURCE_DIR}/ChartLegend.h
  ${CHARTCTRL_SOURCE_DIR}/ChartLineSerie.cpp
  ${CHARTCTRL_SOURCE_DIR}/ChartLineSerie.h
  ${CHARTCTRL_SOURCE_DIR}/ChartLogarithmicAxis.cpp
  ${CHARTCTRL_SOURCE_DIR}/ChartLogarithmicAxis.h
  ${CHARTCTRL_SOURCE_DIR}/ChartMouseListener.h
  ${CHARTCTRL_SOURCE_DIR}/ChartPointsArray.h
  ${CHARTCTRL_SOURCE_DIR}/ChartPointsArray.inl
  ${CHARTCTRL_SOURCE_DIR}/ChartPointsSerie.cpp
  ${CHARTCTRL_SOURCE_DIR}/ChartPointsSerie.h
  ${CHARTCTRL_SOURCE_DIR}/ChartScrollBar.cpp
  ${CHARTCTRL_SOURCE_DIR}/ChartScrollBar.h
  ${CHARTCTRL_SOURCE_DIR}/ChartSerie.cpp
  ${CHARTCTRL_SOURCE_DIR}/ChartSerie.h
  ${CHARTCTRL_SOURCE_DIR}/ChartSerieBase.h
  ${CHARTCTRL_SOURCE_DIR}/ChartSerieBase.inl
  ${CHARTCTRL_SOURCE_DIR}/ChartSeriesMouseListener.h
  ${CHARTCTRL_SOURCE_DIR}/ChartStandardAxis.cpp
  ${CHARTCTRL_SOURCE_DIR}/ChartStandardAxis.h
  ${CHARTCTRL_SOURCE_DIR}/ChartString.h
  ${CHARTCTRL_SOURCE_DIR}/ChartSurfaceSerie.cpp
  ${CHARTCTRL_SOURCE_DIR}/ChartSurfaceSerie.h
  ${CHARTCTRL_SOURCE_DIR}/ChartTitle.cpp
  ${CHARTCTRL_SOURCE_DIR}/ChartTitle.h
  ${CHARTCTRL_SOURCE_DIR}/ChartXYSerie.cpp
  ${CHARTCTRL_SOURCE_DIR}/ChartXYSerie.h
  ${CHARTCTRL_SOURCE_DIR}/PointsOrdering.h

# Add executable to build.
add_executable(${PROJECT_NAME} 
   ${SRC_FILES}
)

if(MSVC)
   target_link_libraries(${PROJECT_NAME} setupapi.lib SqliteLib\sqlite3.lib )
endif(MSVC)
