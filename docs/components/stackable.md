
# 简介
>`stackable` 组件提供了在饥荒游戏中管理实体堆叠功能。

# 属性

- `stacksize`：此属性管理实体的当前堆叠数量。
- `maxsize`：此属性表示实体的最大堆叠数量。
- `ondestack`：从堆叠实体中分离实体时调用的钩子函数，参数是分离出来的实体。

# 属性观察者

## onstacksize
用于多人联机网络同步

## onmaxsize
用于多人联机网络同步

# 方法

## IsStack

**作用**: 检查实体是否已经是堆叠状态。

**参数**: 无

**返回值**:
- **`result`** (boolean): 如果实体已经是堆叠状态返回 `true`，否则返回 `false`。

**代码示例**
```lua
-- 假设有一个可堆叠的实体 inst
local stackable = inst.components.stackable
if stackable:IsStack() then
    print("哇哦，你居然有 "..stackable:StackSize().." 个")
else
    print("你只有一个哦~")
end
```

## StackSize

**作用**: 获取实体当前的堆叠数量。

**参数**: 无

**返回值**:
- **`result`** (number): 实体当前的堆叠数量。

**代码示例**
```lua
-- 假设有一个可堆叠的实体 inst
local stackable = inst.components.stackable
if stackable:IsStack() then
    print("哇哦，你居然有 "..stackable:StackSize().." 个")
else
    print("你只有一个哦~")
end
```

## IsFull

**作用**: 检查实体是否已经达到最大堆叠数量。

**参数**: 无

**返回值**:
- **`result`** (boolean): 如果实体已经达到最大堆叠数量返回 `true`，否则返回 `false`。

**代码示例**
```lua
-- 假设有一个可堆叠的实体 inst
local stackable = inst.components.stackable
if stackable:IsFull() then
    print("不能更多了！！！")
else
    print("继续努力~")
end
```

## SetOnDeStack

**作用**: 设置从堆叠实体中分离实体时调用的钩子函数。

**参数**:
- **`fn`** (function): 从堆叠实体中分离实体时调用的钩子函数。

**返回值**: 无

**代码示例**
```lua
-- 假设有一个可堆叠的实体 inst
local stackable = inst.components.stackable
stackable.SetOnDeStack(function(inst2)
    print("分离出<"..inst2.prefab.."><"..inst2.components.stackable:StackSize()..">个")
end)
```

## SetStackSize

**作用**: 设置实体的堆叠数量。

**参数**:
- **`sz`** (number): 堆叠数量。

**返回值**: 无

**代码示例**
```lua
-- 假设有一个可堆叠的实体 inst
local stackable = inst.components.stackable
-- 在原有数量上增加3个，可超出最大堆叠数量
stackable:SetStackSize(stackable:StackSize() + 3)
-- 设置为3个
stackable:SetStackSize(3)
```

## Get

**作用**: 从堆叠的实体中获取指定数量的实体。

**参数**:
- **`num`** (number): 堆叠数量，可选参数，默认1。

**返回值**: 
- **`inst`** (Entity): 实体，堆叠数量是`num`个。

**代码示例**
```lua
-- 在很多 prefab 里面都有这么一句，
inst.components.stackable:Get():Remove()

```

## RoomLeft

**作用**: 获取最大堆叠数量与当前堆叠数量的差值。

**参数**: 无
**返回值**:
- **`num`** (number): 数量。

**代码示例**
```lua
-- 假设有一个可堆叠的实体 inst
local stackable = inst.components.stackable
stackable:ListenForEvent("stacksizechange", function(stacksize, oldstacksize)
    print("当前堆叠数量："..stacksize.."，新增："..stacksize-oldstacksize)
    print("距离最大堆叠数量还差："..stackable:RoomLeft())
end)
```

## Put

**作用**: 堆叠实体。

**参数**: 
- **`item`** (Entity): 实体。
- **`source_pos`** (Point): 位置。

**返回值**:
- **`ret`** (Entity|nil): 实体。

**代码示例**
```lua
-- 实体木头生成后，尝试合并范围内存在的其他木头
AddPrefabPostInit("log", function(inst)
    -- 堆叠合并
    local function MergeStacks(inst)
        -- 能堆叠的前提条件
        local function CanMergeTestFn(item)
            return item.prefab == inst.prefab
                    and item.skinname == inst.skinname
                    and (item.components.inventoryitem ~= nil and item.components.inventoryitem.is_landed)
                    and (item.components.stackable ~= nil and (item.components.stackable:RoomLeft() >= inst.components.stackable:StackSize()))
        end

        -- 查找范围内的其他实体
        local item = FindEntity(inst, 10, function(item) return CanMergeTestFn(item) end)
        if item ~= nil then
            -- 将查找到的实体合并堆叠到新生成的
            inst.components.stackable:Put(item)
        end
    end

    --尝试合并范围内存在的其他同类实体
    inst:DoTaskInTime(0.1, MergeStacks)
end)
```
