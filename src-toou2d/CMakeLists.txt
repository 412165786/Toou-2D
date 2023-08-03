cmake_minimum_required(VERSION 3.1)

project(Toou2D)
set(LIB_NAME Toou2D)
set(LIB_VERSION 1.0)

set(OUT_PATH  ${PROJECT_SOURCE_DIR}/bin)
aux_source_directory(. SRC_1)
aux_source_directory(./controls/gadget SRC_2)
aux_source_directory(./controls/interface SRC_3)
aux_source_directory(./theme SRC_4)

add_compile_definitions(-D VERSION_IN="1.0.1")
add_compile_definitions(-D URI_STR="Toou2D")

if(${CMAKE_BUILD_TYPE} STREQUAL "Debug")
    set(PROJECT_NAME ${PROJECT_NAME}d)
endif()

add_library(${PROJECT_NAME} SHARED ${SRC_1} ${SRC_2} ${SRC_3} ${SRC_4} ./t2d_res.qrc)
                    
target_link_libraries(${PROJECT_NAME} Qt5::Core Qt5::Quick Qt5::Widgets)

#拷贝 qmldir 到 输出目录
add_custom_target(${PROJECT_NAME}_QMLFILE ALL
    COMMAND
        ${CMAKE_COMMAND} -E 
        copy
        ${CMAKE_CURRENT_SOURCE_DIR}/build-preset/qmldir
        $<TARGET_FILE_DIR:${PROJECT_NAME}>/${PROJECT_NAME}/qmldir   
    COMMAND
        ${CMAKE_COMMAND} -E 
        copy
        ${CMAKE_CURRENT_SOURCE_DIR}/build-preset/plugin.qmltypes
        $<TARGET_FILE_DIR:${PROJECT_NAME}>/${PROJECT_NAME}/plugin.qmltypes
)
#生成plugin.qmltypes qmlplugindump xx.xx 1.0 ./ >  ./xxxx/plugin.qmltypes
# add_custom_target(${PROJECT_NAME}_PLUGIN_INFO ALL
#     qmlplugindump -nonrelocatable ${PROJECT_NAME} ${LIB_VERSION}  > $<TARGET_FILE_DIR:${PROJECT_NAME}>/${LIB_NAME}/plugin.qmltypes
#     DEPENDS ${PROJECT_NAME}
# )