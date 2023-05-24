---
comments: true
---


> 本文的函数定义在游戏脚本跟目录下的`mathutil.lua`，在Mod以外的环境可以直接使用，在Mod环境(即modmain,modmainworldgen,和通过modimport导入的文件)下，需要通过GLOBAL调取。这些函数是一些数学工具函数，能够帮助你完成一些数学相关的计算


## 函数列表

{{ read_csv('./data/mathutil.csv') }}


## GetSineVal
**作用**: 基于游戏时间返回一个正弦波的值。

**参数**
- `mod` (number, 可选): 修改正弦波周期的参数。
- `abs` (boolean, 可选): 是否返回正弦波的绝对值。
- `inst` (object, 可选): 如果提供，将使用这个对象的存活时间而不是游戏时间。

**返回值**: 正弦波的当前值。

**代码示例**
```lua
local mod = 2
local abs = true
local val = GetSineVal(mod, abs)
```

## Lerp
**作用**: 在数值a和b之间根据参数t进行线性插值。

**参数**
- `a` (number): 范围的开始值。
- `b` (number): 范围的结束值。
- `t` (number): 插值参数，通常在0和1之间。

**返回值**: 插值结果。

**代码示例**
```lua
local a = 0
local b = 10
local t = 0.5
local result = Lerp(a, b, t) -- 结果为5
```

## Remap
**作用**: 将一个值从一个范围映射到另一个范围。

**参数**
- `i` (number): 需要被重新映射的值。
- `a` (number): 原始范围的起始值。
- `b` (number): 原始范围的结束值。
- `x` (number): 新范围的起始值。
- `y` (number): 新范围的结束值。

**返回值**: 重新映射后的值。

**代码示例**
```lua
local i = 5
local a = 0
local b = 10
local x = 0
local y = 100
local result = Remap(i, a, b, x, y) -- 结果为50
```

## RoundBiasedUp
**作用**: 对一个数进行四舍五入，0.5的值总是向上舍入。

**参数**
- `num` (number): 需要四舍五入的数字。
- `idp` (number, 可选): 要保留的小数位数。

**返回值**: 四舍五入后的数字。

**代码示例**
```lua
local num = 3.5
local idp = 0
local result = RoundBiasedUp(num, idp) -- 结果为4
```


## RoundBiasedDown
**作用**: 对一个数进行四舍五入，0.5的值总是向下舍入。

**参数**
- `num` (number): 需要四舍五入的数字。
- `idp` (number, 可选): 要保留的小数位数。

**返回值**: 四舍五入后的数字。

**代码示例**
```lua
local num = 3.5
local idp = 0
local result = RoundBiasedDown(num, idp) -- 结果为3
```

## RoundToNearest
**作用**: 将一个数四舍五入到最近的"multiple"的倍数。

**参数**
- `numToRound` (number): 需要四舍五入的数字。
- `multiple` (number): 将数字四舍五入到这个参数的倍数。

**返回值**: 四舍五入后的数字。

**代码示例**
```lua
local numToRound = 13
local multiple = 5
local result = RoundToNearest(numToRound, multiple) -- 结果为15
```
## math.clamp 或 Clamp 
**作用**: 限制一个数在一个范围内。

**参数**
- `num` (number): 需要被限制的数字。
- `min` (number): 范围的最小值。
- `max` (number): 范围的最大值。

**返回值**: 如果数字在范围内，则返回数字本身；如果数字小于最小值，则返回最小值；如果数字大于最大值，则返回最大值。

**代码示例**
```lua
local num = 15
local min = 10
local max = 20
local result = Clamp(num, min, max) -- 结果为15
```

## IsNumberEven
**作用**: 检查一个数是否为偶数。

**参数**
- `num` (number): 需要检查的数字。

**返回值**: 如果是偶数则返回 true，否则返回 false。

**代码示例**
```lua
local num = 4
local result = IsNumberEven(num) -- 结果为 true
```



## DistXYSq
**作用**: 计算两个点在XY平面上的距离的平方。

**参数**
- `p1` (table): 第一个点的坐标，形如 {x = x1, y = y1}。
- `p2` (table): 第二个点的坐标，形如 {x = x2, y = y2}。

**返回值**: 两个点在XY平面上的距离的平方。

**代码示例**
```lua
local p1 = {x = 1, y = 1}
local p2 = {x = 4, y = 5}
local result = DistXYSq(p1, p2) -- 结果为 25
```


## DistXZSq
**作用**: 计算两个点在XZ平面上的距离的平方。

**参数**
- `p1` (table): 第一个点的坐标，形如 {x = x1, z = z1}。
- `p2` (table): 第二个点的坐标，形如 {x = x2, z = z2}。

**返回值**: 两个点在XZ平面上的距离的平方。

**代码示例**
```lua
local p1 = {x = 1, z = 1}
local p2 = {x = 4, z = 5}
local result = DistXZSq(p1, p2) -- 结果为 25
```


## math.range
**作用**: 生成一个指定范围和步长的数列。

**参数**
- `start` (number): 数列的起始值。
- `stop` (number): 数列的结束值。
- `step` (number): 数列的步长。默认为1。

**返回值**: 生成的数列（table）。

**代码示例**
```lua
local start = 1
local stop = 10
local step = 2
local result = math.range(start, stop, step) -- 结果为 {1, 3, 5, 7, 9}
```


## ReduceAngle
**作用**: 对一个角度进行调整，使其值落在 -180 到 180 之间。

**参数**
- `rot` (number): 需要进行调整的角度。

**返回值**: 调整后的角度值。

**代码示例**
```lua
local rot = 360
local result = ReduceAngle(rot) -- 结果为 0
```

## DiffAngle
**作用**: 计算两个角度的绝对差值，返回的差值在0-180之间。

**参数**
- `rot1` (number): 第一个角度值。
- `rot2` (number): 第二个角度值。

**返回值**: 两个角度的绝对差值。

**代码示例**
```lua
local rot1 = 90
local rot2 = 180
local result = DiffAngle(rot1, rot2) -- 结果为 90
```


## ReduceAngleRad
**作用**: 对一个角度进行调整，使其值落在 -π 到 π 之间。

**参数**
- `rot` (number): 需要进行调整的角度，单位为弧度。

**返回值**: 调整后的角度值。

**代码示例**
```lua
local rot = 2 * math.pi
local result = ReduceAngleRad(rot) -- 结果为 0
```


## DiffAngleRad
**作用**: 计算两个角度的绝对差值，返回的差值在0-π之间。

**参数**
- `rot1` (number): 第一个角度值，单位为弧度。
- `rot2` (number): 第二个角度值，单位为弧度。

**返回值**: 两个角度的绝对差值。

**代码示例**
```lua
local rot1 = math.pi / 2
local rot2 = math.pi
local result = DiffAngleRad(rot1, rot2) -- 结果为 π / 2
```