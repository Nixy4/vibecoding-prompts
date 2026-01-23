```c
//stdlib
#include <stdio.h>
#include <stdarg.h>
#include <string.h>
//idf-component
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
//偏应用层工具类
#include "cJSON.h"
//registry-component
#include "esp_camera.h"
#include "lvgl.h"
// project-component
#include "xxxx.h"
```

