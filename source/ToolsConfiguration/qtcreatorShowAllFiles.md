# 在qtcreator中显示所有文件

在CMakeLists.txt中添加如下code

```
FILE(GLOB_RECURSE extra_files ${CMAKE_SOURCE_DIR}/*)
add_custom_target(dummy_${PROJECT_NAME} SOURCES ${extra_files})
```
