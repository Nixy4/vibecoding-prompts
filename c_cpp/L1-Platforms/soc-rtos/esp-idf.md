# 组件CMakeLists

```cmake
set(SRC_DIRS "dir1" "dir2" "dir3")
set(INCLUDE_DIRS "dir1" "dir2" "dir3")
set(REQUIRES_ESP_IDF "idf-component1" "idf-component2" "idf-component3")
set(REQUIRES_ESP_REGISTRY "registry-component1" "registry-component2" "registry-component3")
set(REQUIRES_PROJECT "project-component1" "project-component2" "project-component3")
set(REQUIRES ${REQUIRES_ESP_IDF} ${REQUIRES_ESP_REGISTRY} ${REQUIRES_PROJECT})
set(PRIV_REQUIRES "component1" "component2" "component3")
set(EMBED_FILES "file1" "file2" "file3")
idf_component_register(
  SRC_DIRS ${SRC_DIRS}
  INCLUDE_DIRS ${INCLUDE_DIRS}
  REQUIRES ${REQUIRES}
  PRIV_REQUIRES ${PRIV_REQUIRES}
  EMBED_FILES ${EMBED_FILES}
  )
```

# 头文件规则

以下排列顺序从上往下对应从底层往上层, 编写代码时写上< //* >开头的注释, 其他注释作为提示词给你, 不需要写到代码中, 写出的头文件仅供给你做分类参考, 不是每个文件都必须添加

```c
//*c/cpp stdlib
//由工具链提供的头文件
#include <stdio.h>
#include <stdarg.h>
#include <string.h>
#include <string>
#include <mutex>

//*idf-component
//由esp-idf环境提供的头文件
//esp_ 开头的底层工具
#include "esp_err.h"
#include "esp_log.h"
#include "esp_event.h"
#include "esp_http_server.h"
//soc 和 idf 特性部分
#include "nvs_flash.h"
#include "event_group.h"
#include "wifi.h"
//driver部分
#include "driver/gpio"
#include "driver/i2c_master.h"
#include "driver/spi_master.h"
//偏应用层工具
#include "cJSON.h"

//*registry-component
//esp官方在线仓库提供的非IDF内置组件,由idf_components.yml配置的,存放在 ./managed_components中的
#include "esp_camera.h"
#include "lvgl.h"

//*project-component
//当前工程的离线管理组件, 存放在./components中
#include "xxxx.h"
```

# 