set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake/Modules/")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")

find_package(Boost COMPONENTS REQUIRED
        filesystem
        system
        thread
        program_options
        regex
        )
if (NOT Boost_FOUND)
    MESSAGE(FATAL_ERROR "Could not find boost library")
endif ()

find_package(GraphicsMagick REQUIRED)
if (NOT MAGICK_FOUND)
    MESSAGE(FATAL_ERROR "Could not find GraphicsMagick library")
endif ()

find_package(TBB REQUIRED)
if (NOT TBB_FOUND)
    MESSAGE(FATAL_ERROR "Could not find TBB library")
endif ()

find_package(Tesseract)
if (NOT Tesseract_FOUND)
    MESSAGE(FATAL_ERROR "Could not find Tesseract library")
endif ()

find_package(Poppler COMPONENTS cpp)
if (NOT POPPLER_FOUND)
    MESSAGE(FATAL_ERROR "Could not find Poppler library")
endif ()

include_directories(${MAGICK_INCLUDE_DIR})
include_directories(${TBB_INCLUDE_DIRS})
include_directories(${Tesseract_INCLUDE_DIR})
include_directories(${POPPLER_INCLUDE_DIRS})

link_directories(${MAGICK++_LIBRARY_DIR})

set(SOURCE_FILES main.cxx TesseractWrapper.cxx PixWrapper.cxx Timer.cxx Poppler.cxx PDF.cxx Commandline.cxx Options.cxx PDFPage.cxx)
add_executable(potoo ${SOURCE_FILES})

target_link_libraries(potoo ${MAGICK++_LIBRARIES} ${Boost_LIBRARIES} ${TBB_LIBRARIES} ${Tesseract_LIBS} ${POPPLER_LIBRARIES})

install(TARGETS potoo DESTINATION /usr/local/bin)
