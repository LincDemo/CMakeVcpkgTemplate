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
> ./vcpkg/vcpkg install # 注意一下：他默认是x64-widnows，如果你用vs生成器那没问题。如果你用的ninjaa生成器，`.\vcpkg\vcpkg.exe install --triplet=x64-mingw-dynamic` (或者在清单文件上加?)

> mkdir build && cd build
> cmake .. -DCMAKE_TOOLCHAIN_FILE=../vcpkg/scripts/buildsystems/vcpkg.cmake # 如果不行就换绝对路径，如: cmake .. -DCMAKE_TOOLCHAIN_FILE=-DCMAKE_TOOLCHAIN_FILE=H:/<path>/CMakeVcpkgTemplate/vcpkg/scripts/buildsystems/vcpkg.cmake
> cmake --build .
```

## 可选文件补充说明

### CMakePresets.json 文件

不用这个文件也行：

像CLion、VS 以前都可以在设置中进行配置（但不跨IDE，存在 `.idea`/`.vs` 中，不通用），在设置中的CMake中加上选项：

`-DCMAKE_TOOLCHAIN_FILE=./vcpkg/scripts/buildsystems/vcpkg.cmake`

使用这个文件：

这个文件一是VSCode的 CMake/CMake Tool 插件在使用，二是现在许多新版本的IDE都能支持这个文件（CLion要25版本才支持），更方便你去跨平台使用

### CMakePresets.json 文件不起效的替代方案

VS、VSCode那边支持度挺好，这边说一下CLion-MinGW这样

CMake设置里可以创建两个环境，ninja和vs。前者用MinGW工具链+Ninja生成器，后者用VS工具链+VS生成器，两者的CMake option都要加上 `-DCMAKE_TOOLCHAIN_FILE=./vcpkg/scripts/buildsystems/vcpkg.cmake` (不行就绝对路径)

### github工作流

略，这里我暂时借用 CppCMakeVcpkgTemplate 的内容

## FAQ

- 有的环境可以，但有的环境不行
  - 特别点名 CLion 旧版还不支持 CMake presets
  - 重点关注各种IDE的实际输出，比较他们的值。至于缺省值，可以通过 `cmake --help` 查看
  - widnows特别重点一下他默认Generators是VS还是Ninja(MinGW)，我这里就踩过坑，研究了很久才发现这个问题
  - 可以统一一下CMake默认变量，

- 为什么我的CLion不支持？
  CLion得非常新才支持，https://www.jetbrains.com/help/clion/cmake-presets.html 他这个文章写于10月03。我写这里的时候用的24.3版本不支持，还得更新用beta版才行

- Ninja和VS生成器使用vcpkg分别不行或行。
  - 主要是CLion现版本对这两支持太差了，见：https://www.jetbrains.com/help/clion/cmake-presets.html， https://www.jetbrains.com/help/clion/package-management.html#troubleshooting-mingw
    旧版本一是不支持CMakePresets，二是没有后者的那个修复功能
  - 注意的是vcpkg默认生成的是 `X64-Windows`, 如果没有那个自动修复功能
