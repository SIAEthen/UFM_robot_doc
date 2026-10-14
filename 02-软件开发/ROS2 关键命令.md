## colcon

### 编译

单个包编译

`colcon build --packages-select <name-of-pkg>`

同时编译依赖

`colcon build --packages-up-to <name-of-pkg>`

### 创建包

`ros2 pkg create --build-type ament_cmake <pkg_name>`

## ros2

`ros2 topic hz <topic_name>`