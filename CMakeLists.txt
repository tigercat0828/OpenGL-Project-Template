cmake_minimum_required(VERSION 3.0)

project(OpenGLTemplate)

set(CMAKE_CXX_STANDARD 17)
set(PROGRAM_NAME main)
set(LIBRARY_DIR "${CMAKE_CURRENT_SOURCE_DIR}/ThirdParty/lib")
set(ASSET_DIR "${CMAKE_CURRENT_SOURCE_DIR}/Assets")


include_directories(SYSTEM include)
include_directories(SYSTEM ${CMAKE_CURRENT_SOURCE_DIR}/ThirdParty/include)
include_directories(SYSTEM ${CMAKE_CURRENT_SOURCE_DIR}/ThirdParty/include/imgui)

file(GLOB_RECURSE SOURCES "src/*.cpp" "src/*.c")


add_executable(${PROGRAM_NAME} ${SOURCES})
# SDL2
target_link_libraries(${PROGRAM_NAME} 
	${LIBRARY_DIR}/SDL2/SDL2.lib
	${LIBRARY_DIR}/SDL2/SDL2main.lib
	${LIBRARY_DIR}/spdlog/spdlog.lib
	${LIBRARY_DIR}/glm/glm.lib
) 


# 遍歷所有資產檔案
file(GLOB_RECURSE ASSETS "${ASSET_DIR}/*")

foreach(ASSET ${ASSETS})
    # 計算相對於資源目錄的路徑
    file(RELATIVE_PATH RELATIVE_ASSET_PATH "${ASSET_DIR}" "${ASSET}")
    
    # 設定目標路徑，並保留目錄結構
    set(TARGET_RELEASE_PATH "${CMAKE_BINARY_DIR}/Release/Assets/${RELATIVE_ASSET_PATH}")
    set(TARGET_DEBUG_PATH "${CMAKE_BINARY_DIR}/Debug/Assets/${RELATIVE_ASSET_PATH}")
    
    # 複製檔案到目標路徑
    add_custom_command(
        OUTPUT "${TARGET_RELEASE_PATH}"
        COMMAND ${CMAKE_COMMAND} -E copy "${ASSET}" "${TARGET_RELEASE_PATH}"
        DEPENDS "${ASSET}"
    )
    add_custom_command(
        OUTPUT "${TARGET_DEBUG_PATH}"
        COMMAND ${CMAKE_COMMAND} -E copy "${ASSET}" "${TARGET_DEBUG_PATH}"
        DEPENDS "${ASSET}"
    )
    
    # 加入到目標
    add_custom_target(CopyAssets ALL DEPENDS "${TARGET_RELEASE_PATH}" "${TARGET_DEBUG_PATH}")
endforeach()

# copy SDL2.dll to executable file directory
file(COPY ${LIBRARY_DIR}/SDL2/SDL2.dll DESTINATION ${CMAKE_BINARY_DIR}/Release)
file(COPY ${LIBRARY_DIR}/SDL2/SDL2.dll DESTINATION ${CMAKE_BINARY_DIR}/Debug)
