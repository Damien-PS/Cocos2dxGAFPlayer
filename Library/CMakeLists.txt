# this CMakeLists is to generate gafplayer.a/gafplayer.lib

cmake_minimum_required(VERSION 2.6)

# project(GAFPlayer)

set(COCOS_ROOT ../../cocos2d-x/cocos)
set(GAF_ROOT .)

if(WIN32)
  add_definitions(-D_WINDOWS)
  set(PLATFORM_FOLDER win32)
  set(GAF_PLATFORM_INCLUDE_DIR
    ${COCOS_ROOT}/../external/${PLATFORM_FOLDER}-specific/gles/include/OGLES
  )
elseif(IOS)
  add_definitions(-DCC_TARGET_OS_IPHONE)
  set(PLATFORM_FOLDER ios)
elseif(MACOSX OR APPLE)
  set(PLATFORM_FOLDER mac)
elseif(LINUX)
  add_definitions(-DLINUX)
  set(PLATFORM_FOLDER linux)
elseif(ANDROID)
  add_definitions(-DANDROID)
  set(PLATFORM_FOLDER android)
else()
endif()

add_definitions(-DUSE_FILE32API)
add_definitions(-DCOCOS2D_DEBUG=1)
add_definitions(-DCC_ENABLE_CHIPMUNK_INTEGRATION=0)
add_definitions(-DCC_ENABLE_SCRIPT_BINDING=0)
add_definitions(-DCC_USE_PHYSICS=0)
add_definitions(-DCC_LUA_ENGINE_DEBUG=0)
add_definitions(-DCC_USE_3D_PHYSICS=0)
add_definitions(-DCC_ENABLE_BULLET_INTEGRATION=0)
add_definitions(-DCC_USE_NAVMESH=0)
add_definitions(-DCC_USE_TIFF=0)
add_definitions(-DCC_USE_WEBP=0)
add_definitions(-DRAPIDJSON_HAS_STDSTRING=1)

# set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fno-exceptions -std=c++11 -Wno-deprecated-declarations -Wno-reorder")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fno-exceptions -std=c++11")

if(MACOSX OR APPLE)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -stdlib=libc++")
endif()

include_directories(
  /usr/include/
  /usr/local/include/
  /System/Library/Frameworks/
  ${GAF_PLATFORM_INCLUDE_DIR}
  ${GAF_ROOT}/Sources
  ${COCOS_ROOT}
  ${COCOS_ROOT}/2d
  ${COCOS_ROOT}/platform/${PLATFORM_FOLDER}
  ${COCOS_ROOT}/../external/${PLATFORM_FOLDER}-specific/zlib/include
  ${COCOS_ROOT}/../external/glfw3/include/${PLATFORM_FOLDER}
  ${COCOS_ROOT}/../external
  ${COCOS_ROOT}/base
  ${COCOS_ROOT}/math
)

set(CMAKE_EXE_LINKER_FLAGS �-framework AudioToolbox -framework CoreGraphics -framework QuartzCore -framework UIKit -framework OpenGLES�)

#file(GLOB_RECURSE gaf_source_files
#  "${GAF_ROOT}/Sources/*.cpp")

#add_library(gafplayer ${gaf_source_files})

add_library(gafplayer

            Sources/GAFAnimationFrame.cpp
            Sources/GAFAnimationSequence.cpp
            Sources/GAFAsset.cpp
            Sources/GAFAssetTextureManager.cpp
            Sources/GAFCachedTexture.cpp
            Sources/GAFFile.cpp
            Sources/GAFFilterData.cpp
            Sources/GAFFilterManager.cpp
            Sources/GAFLoader.cpp
            Sources/GAFMask.cpp
            Sources/GAFMovieClip.cpp
            Sources/GAFObject.cpp
            Sources/GAFPrecompiled.cpp
            Sources/GAFQuadCommand.cpp
            Sources/GAFShaderManager.cpp
            Sources/GAFSoundInfo.cpp
            Sources/GAFSprite.cpp
            Sources/GAFStream.cpp
            Sources/GAFSubobjectState.cpp
            Sources/GAFTextData.cpp
            Sources/GAFTextField.cpp
            Sources/GAFTextureAtlas.cpp
            Sources/GAFTextureAtlasElement.cpp
            Sources/GAFTimeline.cpp
            Sources/GAFTimelineAction.cpp
            Sources/PrimitiveDeserializer.cpp
            Sources/TagDefineAnimationFrames.cpp
            Sources/TagDefineAnimationFrames2.cpp
            Sources/TagDefineAnimationMasks.cpp
            Sources/TagDefineAnimationObjects.cpp
            Sources/TagDefineAtlas.cpp
            Sources/TagDefineAtlas3.cpp
            Sources/TagDefineNamedParts.cpp
            Sources/TagDefineSequences.cpp
            Sources/TagDefineSounds.cpp
            Sources/TagDefineStage.cpp
            Sources/TagDefineTextField.cpp
            Sources/TagDefineTimeline.cpp
            )

#install .a file
INSTALL(
  TARGETS gafplayer
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  COMPONENT library
)

#install header files
install(DIRECTORY Sources/ DESTINATION "include/GAFPlayer"
  FILES_MATCHING PATTERN "*.h")


target_include_directories(gafplayer PUBLIC
    Sources
    ${COCOS_ROOT}/../external)