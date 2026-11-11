
## 目录
- 寻求帮助的途径 28
- 本书网站 28
- 邮件列表 28
- IRC 29
- OpenLayers 源代码仓库 29
- 总结 30
## 第 2 章：使用 Firebug 消除错误 31
- Firebug 是什么？ 32
- 设置 Firebug 32
- 动手实践 – 下载 Firebug 32
- Firebug 控制 34
  - 面板 34
    - 控制台面板 35
    - HTML 面板 35
    - CSS 面板 37
    - 脚本面板 37
    - DOM 面板 38
    - 网络面板 38
- 面板结论 41
- 使用控制台面板 42
- 动手实践 – 在控制台中执行代码 42
- 动手实践 – 创建对象字面量 43
  - 对象字面量 44
- 动手实践 – 与地图交互 45
  - API 文档 47
- 总结 47
## 第 3 章：OpenLayers 中的“图层” 49
- 什么是图层？ 50
- OpenLayers 中的图层 50
  - 底图图层 51
  - 叠加图层 51
- 动手实践 – 创建多图层地图 51
- 创建图层对象 54
  - Layer.WMS 类 55
    - WMS 图层参数： 55
  - 参数与参数 57
- 动手实践 – 配置 options 参数 58
- 配置图层选项 61
  - wms_state_lines 图层选项 61
    - 比例尺依赖 61
  - wms_layer_labels 图层选项 62
    - visibility 属性 62
- opacity 属性 62
### 地图瓦片 62
- 多张图像组成地图 63
### 可用的图层属性 65
- 数据类型 66
- OpenLayers.Layer 类属性 66
- 修改图层属性 71
### OpenLayers.Layer 类 71
- 子类 71
- 图层类—子类和父类 72
### 其他图层类型 72
- Layer.ArcGIS93Rest 72
- Layer.ArcIMS 73
- Layer.Google 73
### 动手实践 – 创建 Google Maps 图层 73
- Layer.Grid 75
- Layer.Image 76
### 动手实践 – 使用图像图层 76
- 图像图层参数 77
- Layer.MapGuide 78
- Layer.TileCache 79
- Layer.Vector 79
- Layer.VirtualEarth 79
- Layer.WFS 80
- Layer.WMS 80
- Layer.Yahoo 80
### 访问图层对象 80
### 动手实践 – 访问 map.layers 80
### 动手实践 – 在 Firebug 中访问图层对象 82
- 访问图层属性 82
- map.layers 82
- 存储图层对象的引用 83
### 图层类方法 85
### 动手实践 – 定义全局图层对象变量 85
- 图层类方法定义 86
### 总结 88
## 第 4 章：理解投影 89
### 地图投影 90
- 为什么要使用投影？ 90
- 投影特征 90
- 面积 90
- 比例尺 91
- 形状 91
- 其他特征 91
- 投影类型 92
- EPSG 代码 92
- 动手实践 – 使用不同的投影代码 93
- 指定不同的投影 94
- 经度/纬度 95
- 纬度 96
- 经度 96
- 动手实践 – 确定 LonLat 坐标 96
- OpenLayers 投影类 97
- 创建投影对象 97
- 参数 97
- 函数 98
- 转换投影 99
- 动手实践 – 坐标转换 99
- Proj4js 库 100
- 动手实践 – 设置 Proj4js.org 101
- 定义自定义投影 102
- 总结 102
## 第 5 章：与第三方 API 交互 103
- 第三方地图 API 103
- 地图混搭 104
- OpenLayers 与第三方 API 104
- Google Maps 104
- Google Maps 版本 2 与版本 3 的区别 105
- 动手实践 – 使用 Google Maps V3（标准方式） 105
- 创建 Google Map 图层对象 108
- Google 图层属性 108
- sphericalMercator {Boolean} 109
- type {GmapType} 109
- V2 GMapType 值 110
- 动手实践 – 使用 V2 创建 Google Map 图层（已弃用） 111
- Yahoo! Maps API 113
- 动手实践 – 使用 Yahoo! Maps 图层 113
- Yahoo! Maps 图层类属性 115
- Yahoo! Maps 图层类型 115
- Microsoft 的地图 API 115
- 动手实践 – 创建 Bing/Virtual Earth 图层 115
- VirtualEarth 图层类属性 117
- 可能的 type 值 118
- OpenStreetMap 118
### 动手实践 – 创建 OpenStreetMap 图层 118
- 访问自己的 OSM 瓦片 119
### 球面墨卡托 120
- 球面墨卡托—EPSG 代码 120
### 动手实践 – 使用球面墨卡托 121
- 使用球面墨卡托图层的地图属性 122
  - maxExtent 122
  - maxResolution 122
  - units 123
  - projection 123
### 使用 Google Maps 和其他图层 123
### 动手实践 – 创建你的第一个混搭 124
- 使用球面墨卡托/第三方地图图层的 WMS 127
### 总结 127
## 第 6 章：控制控件 129
### 什么是控件？ 130
- 在 OpenLayers 中使用控件 130
### 向地图添加控件 130
### 动手实践 – 创建一个没有控件的地图 131
### 动手实践 – 向地图添加控件 132
- 通过传入控件数组添加控件 135
- 使用 addControl() 和 addControls() 向地图添加控件 135
- 移除控件 136
### OpenLayers.Control 类 136
- OpenLayers.Control 属性 137
- OpenLayers.Control 函数 137
### OpenLayers.Control 子类 138
- OpenLayers.Control.ArgParser 138
- OpenLayers.Control.Permalink 139
- OpenLayers.Control.Attribution 139
  - Attribution 属性 139
### 动手实践 – 使用署名 140
- OpenLayers.Control.EditingToolbar 141
- OpenLayers.Control.Graticule 141
  - Graticule 属性 142
- OpenLayers.Control.KeyboardDefaults 143
  - KeyboardDefaults 属性 143
- OpenLayers.Control.LayerSwitcher 143
  - LayerSwitcher 属性 143
  - LayerSwitcher 函数 144
- OpenLayers.Control.MousePosition 144
  - MousePosition 属性 144
- OpenLayers.Control.Navigation 145
  - Navigation 属性 145
- OpenLayers.Control.NavigationHistory 146
  - NavigationHistory 属性 146
  - NavigationHistory 函数 146
### 动手实践 – 使用 NavigationHistory 控件 146
- OpenLayers.Control.NavToolbar 147
- OpenLayers.Control.OverviewMap 147
  - OverviewMap 属性 148
  - OverviewMap 函数 150
- OpenLayers.Control.PanPanel 150
  - PanPanel 属性 151
- OpenLayers.Control.PanZoom 151
- OpenLayers.Control.PanZoomBar 151
  - PanZoomBar 属性 151
- OpenLayers.Control.Scale 151
  - Scale 属性 152
- OpenLayers.Control.ScaleLine 152
  - ScaleLine 属性 152
- OpenLayers.Control.ZoomPanel 153
### 面板 153
- 控件类型 153
### 动手实践 – 使用面板 154
- OpenLayers.Control.Panel 157
  - Panel 属性 158
  - Panel 函数 159
- 接下来呢？ 159
### 创建自己的控件 159
- OpenLayers.Control.Button 159
  - Button 属性 160
  - Button 函数 161
### 创建自定义按钮 161
### 动手实践 – 创建一个简单按钮 161
- 其他控件类型 165
  - 创建其他按钮控件类型的流程 165
### 事件 165
- 事件监听器和处理器 165
- 自定义事件 166
- 创建 TYPE_TOGGLE 控件 166
### 动手实践 – 创建自定义 TYPE_TOGGLE 控件 166
### 总结 170
## 第 7 章：样式化控件 171
- 什么是 CSS？ 172
  - CSS 和 HTML 背后的思想 172
  - 编辑 CSS 172
  - HTML 元素 173
    - HTML—ID 和类 173
  - 使用 CSS 样式化 HTML 元素 174
  - 在代码中使用 CSS 175
- 动手实践 – 使用外部 CSS 文件 176
  - 层叠样式表—继承 178
    - 继承顺序 178
    - 引用元素 179
- OpenLayers 与 CSS 180
  - 样式化 OpenLayers—使用主题 180
  - 创建自己的主题 181
    - OpenLayers—类名和 ID 181
- 动手实践 – 样式化控件 182
- 动手实践 – 样式化 LayerSwitcher 控件 186
- 其他资源 188
- 总结 189
## 第 8 章：解析 Map 类 191
- Map 类 192
  - 创建地图对象 192
- Map 类属性 192
  - 地图属性 193
    - allOverlays 193
    - controls 193
    - displayProjection 194
    - div 194
- 动手实践 – 使用 allOverlays Map 属性 194
  - eventListeners 196
  - fallThrough 197
  - layers 197
  - maxExtent 198
  - minExtent 198
  - restrictedExtent 198
  - numZoomLevels 198
- 动手实践 – 设置缩放级别和 maxExtent 199
  - 地图属性—续 200
    - Resolutions 200
- 动手实践 – 使用 resolutions 数组 201
  - 地图/图层属性继承 201
