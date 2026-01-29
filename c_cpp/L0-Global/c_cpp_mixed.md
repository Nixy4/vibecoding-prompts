# C/C++ 混合编程命名规范

本规范旨在解决 C 与 C++ 代码共存时的识别混淆问题。核心原则是**视觉隔离**：通过强制的风格差异，使开发者能一眼分辨出符号的语言来源。

---

## 1. 核心差异速查表

| 维度 | **C 语言风格** (底层/驱动/Legacy) | **C++ 语言风格** (应用/逻辑/封装) | 辨识特征 |
| :--- | :--- | :--- | :--- |
| **文件扩展名** | `.c` / `.h` | `.cpp` / `.hpp` | 扩展名区分 |
| **类型定义** | `snake_case_t` (带 `_t` 后缀) | `PascalCase` (无后缀) | **_t vs Pascal** |
| **函数/方法** | `snake_case` (全小写) | `PascalCase` (大驼峰) | **大小写** |
| **全局变量** | `g_snake_case` | `g_PascalCase` 或 命名空间内 | **后缀/Casing** |
| **常量/宏** | `MACRO_ALL_CAPS` | `kPascalCase` (编译时常量) | **k前缀** |
| **结构体成员** | `snake_case` | `m_snake_case` (私有/保护) | **m_前缀** |

---

## 2. C 语言部分 (遵循 POSIX/Linux 风格)

*适用于：RTOS 内核接口、硬件驱动 (BSP)、跨语言导出的 C API。*

### 2.1 类型 (Types)
强制使用 `snake_case` 并以 `_t` 结尾。
```c
typedef struct {
    int x;
    int y;
} point_2d_t;

typedef enum {
    LED_OFF = 0,
    LED_ON
} led_status_t;
```

### 2.2 函数 (Functions)
强制使用 `snake_case`，必须包含 `模块_` 前缀。
```c
// 格式: <module>_<action>
void led_driver_init(void);
int  sensor_read_data(void);
```

### 2.3 变量 (Variables)
*   **全局**: `g_` + `snake_case` (例如: `g_system_tick`)
*   **文件静态**: `s_` + `snake_case` (例如: `s_is_initialized`)
*   **局部**: `snake_case` (使用简写: `buf`, `len`, `idx`)

---

## 3. C++ 语言部分 (遵循 Google/Unreal 风格)

*适用于：业务逻辑、算法封装、高级设备抽象。*

### 3.1 命名空间 (Namespaces)
使用 `snake_case`，禁止全局污染。
```cpp
namespace vehicle_control {
    // ...
}
```

### 3.2 类型 (Classes/Structs/Enums)
强制使用 `PascalCase`，**严禁**使用 `_t` 后缀。
```cpp
class PidController { ... };
struct MotionVector { ... }; // 纯数据结构
enum class LedState { ... };
```

### 3.3 函数 (Functions & Methods)
**所有** C++ 函数（无论是类方法还是普通函数）均使用 `PascalCase`。
这是区分 C 函数（snake_case）的最强视觉特征。

```cpp
namespace math_utils {
    // C++ 普通函数 -> PascalCase
    double CalculateDistance(Point a, Point b); 
}

class Motor {
public:
    // C++ 成员函数 -> PascalCase
    void SetSpeed(int speed);
};

// 对比：
// led_on();        // 一眼看出是 C 函数
// motor.SetSpeed(); // 一眼看出是 C++ 方法
```

### 3.4 变量 (Variables)
*   **类成员变量**: `m_` + `snake_case` (强提醒这是内部状态)
    ```cpp
    class Session {
    private:
        int m_timeout_ms;
        bool m_is_connected;
    };
    ```
*   **全局/静态变量**: 必须位于命名空间内，建议 `g_PascalCase` 以示区别。
    ```cpp
    namespace config {
        extern int g_MaxRetryCount;
    }
    ```
*   **常量 (const/constexpr)**: 使用 `k` 前缀 + `PascalCase`。
    ```cpp
    const int kDefaultTimeout = 500;
    ```

---

## 4. 混合编程示例 (Mixed Context)

在同一个文件中调用不同的接口时，风格对比非常明显：

```cpp
#include "drivers/uart_driver.h" // C 头文件
#include "sys/Logger.hpp"        // C++ 头文件

// C++ 函数
void SystemStartup() {
    // 1. 初始化 C 驱动 (snake_case)
    uart_config_t cfg;           // C 类型: _t 后缀
    cfg.baud_rate = 115200;      // C 成员: snake_case
    uart_init(&cfg);             // C 函数: snake_case

    // 2. 初始化 C++ 模块 (PascalCase)
    Logger::Init();              // C++ 静态方法: PascalCase
    
    // 3. 常量使用
    const int kMaxBuffer = 1024; // C++ 常量
    char buffer[kMaxBuffer];
    
    // 4. 混合逻辑
    if (g_uart_ready) {          // C 全局变量: g_snake
        sys::LogInfo("Ready");   // C++ 函数: PascalCase
    }
}
```
