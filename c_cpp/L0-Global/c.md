# 全局变量名

- 跨文件访问(在其相关头文件中用extern修饰声明) : snake_case, 并以g_开头
- 文件私有(必须用static修饰): snake_case, 并以s_开头
- *禁止定义既无static修饰又无g_前缀的全局变量*

# 函数入参变量名

snake_case, 避免过度缩写

# 局部变量名

snake_case, 仅使用通用缩写(如 buf, len, idx), 严禁自造生僻缩写

# 函数名

- 整体风格

  - 公共函数/会通过头文件暴露的函数: snake_case
  - 普通函数(没有static/extern修饰): snake_case
  - 静态函数(static修饰): snake_case, 并以单个 '_' 开头

- 字段规划

  - 公共函数(会通过头文件暴露的函数) /普通函数(可能在模块内部其他文件通过extern引入)

    ```c
    //返回值类型 模块名_子模块名(如果有)_动作_操作对象_细分功能(如果有)(...);
    xxx_t i2c_master_get_handle(int i2c_num, i2c_handle_t* handle_ptr);
    xxx_t servo_set_angle_async(int angle);
    xxx_t servo_set_agnle_sync(int angle);
    ```

  - 静态函数(通过static修饰)

    ```c
    //返回值类 _子模块名(如果有)_动作_操作对象_细分功能(如果有)(...);\
    //i2c
    xxx_t _master_check_handle(...);
    void _master_set_status(...);
    ```

# 结构体/联合体/枚举变量

POSIX 风格

