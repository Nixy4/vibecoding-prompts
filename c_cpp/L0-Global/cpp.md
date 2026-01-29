# 命名空间 (Namespaces)

- 全小写 snake_case
- *禁止*使用全局命名空间污染，所有非局部符号都应包含在命名空间中
- 内部实现细节建议使用匿名命名空间 `namespace { ... }`

# 全局变量名

- 避免使用非 const 全局变量
- 必须放在命名空间内
- 全局常量: kPascalCase 或 ALL_CAPS
- 静态/文件私有变量: 放入匿名命名空间

# 函数入参变量名

snake_case, 避免过度缩写

# 局部变量名

snake_case, 仅使用通用缩写(如 buf, len, idx), 严禁自造生僻缩写

# 普通函数名 (Free Functions)

- 位于命名空间中: snake_case (参考 std 风格)
- 避免使用 C 风格的长前缀 (如 module_action), 利用命名空间区分

```cpp
namespace hardware {
namespace i2c {

// hardware::i2c::get_handle(...)
void get_handle(...);

}
}
```

# 类/结构体/枚举类名

PascalCase

# 类/结构体成员变量

snake_case

- private/protected: 后置下划线 (`buffer_size_`)
- public: 无前缀 (通常仅用于 POD 结构体)
- *禁止*使用单下划线前缀 (`_name`)，以免与编译器保留字冲突

# 类/结构体成员函数

- 统一使用 PascalCase (保持作为 Public 接口或内部实现的一致性)
- *不再*通过大小写区分访问权限

```cpp
class ServoController {
public:
    void SetAngleAsync(int angle); // PascalCase
protected:
    void UpdateState();            // PascalCase
private:
    void CalculateOffset();        // PascalCase
    int m_current_angle;           // m_ prefix
};
```

