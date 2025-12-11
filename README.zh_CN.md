# deepin-ocr-plugin-manager

OCR 插件管理器，用于 UOS（统信操作系统）的 OCR 开发库和插件管理工具。

## 简介

deepin-ocr-plugin-manager 是一个 OCR（光学字符识别）插件管理系统，提供了统一的插件接口和管理机制。它支持动态加载和管理多个 OCR 插件，并提供了硬件加速、多语言支持等功能。

## 功能特性

- **插件管理**：支持动态扫描、加载和管理多个 OCR 插件
- **默认插件**：内置基于 PaddleOCR 和 NCNN 的默认 OCR 插件
- **硬件加速**：支持 GPU/Vulkan 硬件加速，提升识别性能
- **多语言支持**：支持中文简体、中文繁体、英文等多种语言
- **多线程处理**：支持多线程并发处理，提高识别效率
- **灵活配置**：支持自定义硬件配置、线程数等参数

## 依赖要求

### 构建依赖

- CMake (>= 3.10)
- debhelper (>= 11)
- libncnn-dev
- libopencv-mobile-dev
- pkg-config

### 运行时依赖

- libncnn
- libopencv-mobile

## 构建安装

### 从源码构建

```bash
mkdir build
cd build
cmake ..
make
sudo make install
```

### Debian 包构建

```bash
dpkg-buildpackage -b
```

## 使用方法

### 基本使用

```cpp
#include <deepinocrplugin.h>

using namespace DeepinOCRPlugin;

// 创建 OCR 驱动实例
DeepinOCRDriver driver;

// 加载默认插件
if (driver.loadDefaultPlugin()) {
    // 设置图片文件
    driver.setImageFile("/path/to/image.png");
    
    // 执行 OCR 识别
    if (driver.analyze()) {
        // 获取识别结果
        std::string result = driver.getAllResult();
        std::vector<TextBox> boxes = driver.getTextBoxes();
    }
}
```

### 加载自定义插件

```cpp
// 获取可用插件列表
auto plugins = driver.getPluginNames();

// 加载指定插件
if (driver.loadPlugin("my-custom-plugin")) {
    // 使用插件进行 OCR 识别
    driver.analyze();
}
```

### 硬件加速配置

```cpp
// 设置使用 GPU 加速
std::vector<std::pair<HardwareID, int>> hardware;
hardware.push_back({HardwareID::GPU_Vulkan, 0});
driver.setUseHardware(hardware);

// 设置最大线程数
driver.setUseMaxThreadsCount(4);
```

### 语言设置

```cpp
// 获取支持的语言列表
auto languages = driver.getLanguageSupport();

// 设置识别语言
driver.setLanguage("zh-Hans_en");  // 中文简体+英文
```

## 项目结构

```
deepin-ocr-plugin-manager/
├── assets/              # 资源文件（OCR 模型等）
│   └── model/           # OCR 模型文件
├── src/                 # 源代码
│   ├── deepinocrplugin.*    # 插件管理器核心代码
│   └── paddleocr-ncnn/      # PaddleOCR 插件实现
├── debian/              # Debian 打包配置
└── CMakeLists.txt       # CMake 构建配置
```

## 支持的插件格式

插件需要实现以下接口：

- `loadPlugin()`: 加载插件
- `unloadPlugin()`: 卸载插件
- `pluginVersion()`: 获取插件版本

插件需要提供 `libload.so` 动态库文件，并实现相应的 OCR 功能接口。

## 许可证

本项目采用 GPL-3.0-or-later 许可证。详见 [LICENSE](LICENSE) 文件。

本项目遵循 [REUSE 规范](https://reuse.software/)，所有许可证信息都清晰标注在源代码文件中。项目使用的许可证包括：

- **GPL-3.0-or-later**: 主要代码
- **MIT**: Debian 打包文件
- **Apache-2.0**: PaddleOCR 相关工具文件
- **BSL-1.0**: Clipper 库

所有许可证的完整文本位于 `LICENSES/` 目录中。

## 贡献

欢迎提交 Issue 和 Pull Request。

## 相关链接

- [项目主页](http://www.deepin.org/)
- [问题反馈](https://github.com/linuxdeepin/deepin-ocr-plugin-manager/issues)

## 维护者

Deepin Packages Builder <packages@deepin.com>

