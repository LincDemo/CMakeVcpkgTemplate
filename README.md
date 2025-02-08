# CMakeVcpkgTemplate

该模板的特点：

- 描述了从零搭建的所有操作，让你知其所以然
- 最简化的版本中，项目创建仅需 `2个文件+5条命令`，项目克隆准备 `3条命令`，运行 `3条命令`
- 同时提供了大量适用于其他IDE或环境的可选文件/操作

## 目录与可选说明

[dir]

- .git/                    | (无需手动管理) git相关
- .github/                 | (可选) 工作流文件
- src/                     | 源码
- vcpkg/                   | (无需手动管理) git相关，子仓库相关
- .clang-format            | (可选) 代码规范相关
- .clang-tidy              | (可选) 代码规范相关
- .gitignore               | (可选) 忽略文件
- .gitmodules              | (无需手动管理) git相关，子仓库相关
- CMakeLists.txt           | CMake构建配置
- CMakePresets.json        | (可选) CMake配置的配置
- README.md                | (可选) 自述文档
- vcpkg.json               | (无需手动管理) vcpkg相关
- vcpkg-configuration.json | (无需手动管理) vcpkg相关
- LICENSE                  | (可选) 协议

## 从零搭建

以下命令组，如果你是windows，将 `&&` 更换成 `;`

Create Project

```bash
> cmake --version
> ninja --version
> gcc --version
> g++ --version
> gdb --version

> mkdir CMakeVcpkgTemplate && cd CMakeVcpkgTemplate

# add files
> git init
> git submodule add https://github.com/microsoft/vcpkg.git vcpkg
> ./vcpkg/bootstrap-vcpkg.bat # or .sh
> ./vcpkg/vcpkg new --application
> ./vcpkg/vcpkg add port fmt
> (add CMakeLists.txt)
> (add main.cpp)

# (optional, choosable)
> (add .gitignore、README.md、LICENSE)

# Use
# git clone 
```

Push、Template Project

```bash
> git add -A
> git commit -m "init"
# Then push according to github prompt
# 然后根据github提示push

# The project can be converted to a template repository in the github setting
# 上github setting中可以将该项目转换为模板存储库
```

Use Project

```bash
> git clone --recursive https://github.com/LincDemo/CMakeVcpkgTemplate.git && cd CMakeVcpkgTemplate
> ./vcpkg/bootstrap-vcpkg.bat
> ./vcpkg/vcpkg install

> mkdir build && cd build
> cmake .. -DCMAKE_TOOLCHAIN_FILE=../vcpkg/scripts/buildsystems/vcpkg.cmake
> cmake --build .
```

## 可选文件补充说明

### CMakePresets.json 文件

不用这个文件也行：

像CLion、VS都可以在设置中进行配置（但不跨IDE，不通用），在CMake配置中加上选项：

`-DCMAKE_TOOLCHAIN_FILE=./vcpkg/scripts/buildsystems/vcpkg.cmake`

使用这个文件：

这个文件一是VSCode的 CMake/CMake Tool 插件在使用，二是现在许多新版本的IDE都能支持这个文件，更方便你去跨平台使用

### github工作流

略，这里我暂时借用 CppCMakeVcpkgTemplate 的内容
