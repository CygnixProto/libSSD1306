cmake_minimum_required(VERSION 3.10)
project(SSD1306)

set(CMAKE_BUILD_TYPE Release)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -std=c++14")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wall")

set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${PROJECT_BINARY_DIR}/lib)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${PROJECT_BINARY_DIR}/bin)

option(BUILDEXAMPLES "Build Example Programs" OFF)
#--------------------------------------------------------------------------

add_library(SSD1306 STATIC lib/FileDescriptor.cxx
						   lib/OledHardware.cxx
						   lib/OledPixel.cxx
						   lib/OledFont8x8.cxx
						   lib/OledFont8x12.cxx
						   lib/OledFont8x16.cxx
						   lib/OledGraphics.cxx
						   lib/OledI2C.cxx)

include_directories(${PROJECT_SOURCE_DIR}/lib)

file(GLOB LIB_PUBLIC_HEADERS ${PROJECT_SOURCE_DIR}/lib/*.h)
install(TARGETS SSD1306 DESTINATION lib)
install(FILES ${LIB_PUBLIC_HEADERS} DESTINATION include/SSD1306)


set(EXTRA_LIBS ${EXTRA_LIBS} HD44780Lcd)

#--------------------------------------------------------------------------

if(BUILD_EXAMPLES)
	add_executable(analog examples/analog.cxx examples/LinuxKeys.cxx)
	target_link_libraries(analog SSD1306)

	add_executable(clock examples/clock.cxx examples/LinuxKeys.cxx)
	target_link_libraries(clock SSD1306)

	add_executable(ipaddress examples/ipaddress.cxx examples/LinuxKeys.cxx)
	target_link_libraries(ipaddress SSD1306)

	add_executable(life examples/life.cxx examples/LinuxKeys.cxx)
	target_link_libraries(life SSD1306)

	add_executable(testbitmap examples/testbitmap.cxx examples/LinuxKeys.cxx)
	target_link_libraries(testbitmap SSD1306)

	add_executable(testoled examples/testoled.cxx examples/LinuxKeys.cxx)
	target_link_libraries(testoled SSD1306)
endif()

