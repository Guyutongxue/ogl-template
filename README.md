# 计算机图形学 "Common" 库使用模板
# OpenGL "Common" Library boilerplate

计算机图形学助教提供了 [huisedenanhai/ogl-intro](https://github.com/huisedenanhai/ogl-intro)，其中的 `common` 库是很有用的。但助教的 CMake 配置有一些古怪，包含了（本不应该包含的）第三方库，而且没有说明 MinGW 的构建方式。本仓库通过使用 Conan 包管理器来解决这些问题。

本仓库不适用于 32 位系统。

模板的代码取自上述仓库的 `05-pbr`。其所需要的数据同样存放在 `data` 中。

在进行以下步骤之前，请先安装 Conan 包管理器（[官网](https://conan.io)）。如果有 Python 的话就从 `pip` 安装。（信科的学生电脑里应该都有吧。）

Of course you should have CMake installed.

## Windows + Visual Studio

测试环境：Windows 10 20H2 + Visual Studio 2019。

先在仓库根目录执行：
```
mkdir build
cd build                    # 切换到构建目录
conan install .. -b missing # 安装第三方库
cmake ..                    # 生成 VS 解决方案
```

> `-b missing` 的含义是：如果 Conan 源找不到已经编译好的二进制文件，则从源码编译。

然后启动 `build/OglTemplate.sln` 即可打开解决方案。

- 编译：`生成->生成解决方案`
- 调试/运行：首先在右侧解决方案资源管理器中将 `main` **设为启动项目**，然后点击“本地 Windows 调试器”即可调试。

## Windows + MinGW

测试环境：Windows 10 20H2 + MinGW with GCC 10.2.0。（网上鲜有这个版本的 MinGW，欢迎前往 [Guyutongxue/mingw_release](https://github.com/Guyutongxue/mingw-release) 下载，或者国内网盘 https://wws.lanzous.com/iAj1Zge4fyd）

> 什么？您用 Dev-C++？那我建议您现在就关掉这个页面。

（不必需）强烈建议安装 VS Code，及其扩展 C/C++、CMake Tools。然后配置 `.vscode/c_cpp_properties.json` 为：
```JSON
{
    "configurations": [
        {
            "name": "Win32",
            "configurationProvider": "ms-vscode.cmake-tools",
            "intelliSenseMode": "gcc-x64",
            "compilerPath": "/path/to/g++", // 自己改一下！
            "cStandard": "c17",
            "cppStandard": "c++17"
        }
    ],
    "version": 4
}
```

接下来执行一样的命令，不过稍微更改
```
mkdir build
cd build                                                   # 切换到构建目录
conan install .. -pr ../conan_mingw_profile.txt -b missing # 注意这里需要指明 profile
cmake .. -G "MinGW Makefiles"                              # 注意这里需要指明 generator
mingw32-make                                               # 最后构建即可
```

其中，`cmake` 命令可以通过 VS Code 的 CMake 插件的“配置所有项目”这个按钮完成（注意需要提前在下面选择工具包为 `GCC 10.2.0`）。

其中，`mingw32-make` 命令可以通过 VS Code 的 CMake 插件的“生成所有项目”这个按钮完成。

如需调试或运行，可用 VS Code 的 CMake 插件的“启动目标”和“启动调试”按钮。直接点一下就成，很快。

> `conan_mingw_profile.txt` 让 Conan 在构建第三方库的时候使用 GCC 而非 MSVC。

## GNU/Linux

测试环境：Ubuntu 20.04 LTS + GCC 10.2.0

你可能需要先安装一些库： `sudo apt install libgl-dev libglu1-mesa-dev`。

```
mkdir build && cd build     # 切换到构建目录
conan install .. -b missing # 安装第三方库
cmake ..                    # 生成 Makefile
make                        # 最后构建即可
```

然后运行 `build/bin/main`. 我不知道为什么，VS Code 的 CMake 扩展提供的启动按钮不好用。

## Mac + AppleClang

测试环境：macOS Big Sur 11.2.3 + AppleClang 12.0.0

```
mkdir build && cd build     # 切换到构建目录
conan install .. -b missing # 安装第三方库
cmake ..                    # 生成 Makefile
make                        # 最后构建即可
```

然后运行 `build/bin/main`. 我仍然不知道为什么，VS Code 的 CMake 扩展提供的启动按钮不好用。

## Mac + Xcode

应该没人用 Xcode 写 C++ 吧。。

```
mkdir build && cd build     # 切换到构建目录
conan install .. -b missing # 安装第三方库
cmake .. -G Xcode           # 生成 Xcode 项目
```

然后启动 `build/OglTemplate.xcodeproj` 即可。

- 构建：`Product->Build`
- 运行：首先在上方选择 `main` 为构建目标，然后 `Product->Run`。

## 跨平台 CLion

测试环境
- macOS Big Sur 11.2.3 + CLion 2020.3 + AppleClang 12.0.0
- Windows 10 20H2 + CLion 2020.3 + MinGW with GCC 10.2.0
- Windows 10 20H2 + CLion 2020.3 + Visual Studio 2019
- Ubuntu 20.04 LTS + CLion 2020.3 + GCC 10.2.0

安装官方的 [Conan 插件](https://plugins.jetbrains.com/plugin/11956-conan)，然后重启 CLion。

（如果你使用 MSVC，需要在设置中将目标架构调整到 `amd64`。）

直接在 CLion 中打开 `ogl-template` 这个文件夹。

点击下方 Conan 面板，首先左侧 Match profile 选择 default。（如果你使用 MinGW ，需在设置中添加参数 `-pr ./conan_mingw_profile.txt`）然后按 Install 安装第三方库。

再点击下方 CMake 面板，左侧 Reload CMake Project 重新配置。

最后，点击上方的编译或运行键即可。

