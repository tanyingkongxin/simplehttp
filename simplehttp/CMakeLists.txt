cmake_minimum_required(VERSION 3.10)

project(SimpleHTTP)

set(LIBEVENT /usr/local CACHE PATH "Path to libevent")
set(TARGET /usr/local CACHE PATH "Installation target directory")

include_directories(
    ${LIBEVENT}/include
  ${CMAKE_CURRENT_SOURCE_DIR}
)

add_compile_options(-Wall -g)

add_library(simplehttp STATIC simplehttp.c async_simplehttp.c timer.c log.c util.c stat.c request.c options.c)

target_link_libraries(simplehttp PRIVATE ${LIBEVENT}/lib/libevent.a m)

add_executable(testserver testserver.c)

# 设置可执行文件的链接选项
target_link_libraries(testserver PRIVATE simplehttp)

# 安装规则
install(TARGETS simplehttp RUNTIME DESTINATION ${TARGET}/bin LIBRARY DESTINATION ${TARGET}/lib ARCHIVE DESTINATION ${TARGET}/lib)
install(FILES simplehttp.h DESTINATION ${TARGET}/include/simplehttp)
install(FILES queue.h uthash.h utlist.h utstring.h utarray.h options.h DESTINATION ${TARGET}/include/simplehttp)

# 清理规则
set_directory_properties(PROPERTIES ADDITIONAL_MAKE_CLEAN_FILES "libsimplehttp.a;testserver")

# 导出CPack配置，以便可以生成安装包
include(CPack)