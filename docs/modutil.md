---
comments: true
---


> 本文的函数定义在游戏脚本跟目录下的modutil.lua，是一系列适用于Mod环境的API，它们是klei专门提供给Mod开发者的，在Mod环境内不需要引用全局变量即可使用。这些API作用非常丰富，掌握好了可以让你的Mod开发水平更上一层楼。

## API列表

其中可用范围的含义，全部是指可以在modmain和modworldgenmain中使用。

{{ read_csv('./data/modutil.csv') }}





## 物品制作

### AddRecipe2

**作用**: 添加一个新的制作配方

**参数**

- `name` (string): 配方的名称。
- `ingredients` (table): 配方所需的原料列表。
- `tech` (string): 解锁该配方所需的科技名称。
- (可选)`config` (table): 一个可选的配置表，包含以下字段: 
    - (可选)`placer`(string): 放置物 prefab，用于显示一个建筑的临时放置物，在放下建筑后就会消失。
    - (可选)`min_spacing` (num): 建造物的最小间距。
    - (可选)`nounlock` (bool): 如果为 false，只能在对应的科技建筑旁制作。否则在初次解锁后，就可以在任意地点制作。
    - (可选)`numtogive` (num): 制作成功后，玩家将获得的物品数量。
    - (可选)`builder_tag` (string): 要求具备的制作者标签。如果人物没有此标签，便无法制作物品，可以用于人物的专属物品。
    - (必须)`atlas` (string): 物品图标所在的 atlas 文件路径，用于制作栏显示图片，其实不填也行，但图标会是空的。
    - (可选)`image` (string): 物品图标的文件名，其实 atlas 中已包含，不必再填。
    - (可选)`testfn` (function): 放置时的检测函数，比如有些建筑对地形有特殊要求，可以使用此函数检测。
    - (可选)`product` (string): 产出物，表示制作成功后产生的物品。默认
    - (可选)`build_mode` (string): 建造模式，必须使用常量表`BUILDMODE`，形如`BUILDMODE.LAND`，具体取值为无限制（NONE）、地上（LAND）和水上（WATER）。
    - (可选)`build_distance` (num): 建造距离，表示玩家与建筑之间的最大距离。
- (可选)**`filters`** (table): 一个可选的过滤器列表，包含要将配方添加到的过滤器名称。


**代码示例**
```lua
-- 文件: modmain.lua
local recipe_name = "lotus_umbrella" -- 配方唯一名称，不可重复，通常用prefab名
local ingredients = {Ingredient("cutgrass", 1), Ingredient("twigs", 1)} -- 原料表
local tech = TECH.NONE -- 所需科技，必须使用常量表TECH的值
local config = {
    atlas = "images/inventoryimages/lotus_umbrella.xml",
}
AddRecipe2(recipe_name, ingredients, tech, config)
STRINGS.RECIPE_DESC.LOTUS_UMBRELLA = "荷叶做的雨伞" -- 制作栏描述
```

### AddCharacterRecipe

**作用**: 添加角色专属制作配方。实际上参数和作用都和AddRecipe2一样，专属体现在config中的builder_tag

**参数**
- **`name`** (string): 配方的名称。
- **`ingredients`** (table): 配方所需的原料列表。
- **`tech`** (string): 解锁该配方所需的科技名称。
- **`(可选) config`** (table): 一个可选的配置表，同AddRecipe2
- **`(可选) filters`** (table): 一个可选的过滤器列表，包含要将配方添加到的额外过滤器名称。

**返回值**: 

- **`rec`** (Recipe2): 创建的 **`Recipe2`** 对象。

**代码示例**
```lua
-- 创建一个角色专属配方，只有可读书的人才能使用
local config2 = {
    atlas = "images/inventoryimages/lotus_umbrella.xml",
    builder_tag = "reader",
    product = "lotus_umbrella", -- 当recipe名不是prefab名时，需要显式指定
}
AddCharacterRecipe("lotus_umbrella_few", {Ingredient("cutgrass", 1)}, TECH.NONE, config2, {"SAMANSHA"})
```


### AddRecipeFilter

**作用**: 添加一个新的配方过滤器。

**参数**

- **`filter_def`** (table): 过滤器定义，包括以下字段: 
    - **`name`** (string): 过滤器的ID。主要用于两处，一是 **`STRINGS.UI.CRAFTING_FILTERS[name]`**。赋值给出在UI界面上显示的过滤器名字，二是在其它API使用过滤器作为参数时用于指代，请参考示例代码。
    - **`atlas`** (string 或 function): 图标的图集，可以是字符串或函数。
    - **`image`** (string 或 function): 显示在制作菜单中的图标，可以是字符串或函数。
    - **`(可选) image_size`** (table): 自定义图像尺寸，默认64。
    - **`(可选) custom_pos`** (table): 自定义的过滤器位置，默认为false。 如果为 true，则过滤器图标不会被添加到网格中，而是把这个分类下的物品都放在mod物品分类下。
    - **`recipes`** (table): *不支持此字段！* 请创建过滤器后，将过滤器传递给 **`AddRecipe2()`** 或 **`AddRecipeToFilter()`** 函数。
- **`(可选) index`** (number): 插入过滤器定义的索引。如果提供，将在指定索引处插入过滤器定义；否则，将过滤器定义添加到列表末尾。

**代码示例**
```lua
-- 加载资源
Assets = {
    Asset("ATLAS", "images/craft_samansha_icon.xml"),
}
-- 定义一个新的过滤器对象
local filter_samansha_def = {
    name = "SAMANSHA",
    atlas = "images/craft_samansha_icon.xml",
    image = "craft_samansha_icon.tex"
}
-- 添加自定义过滤器
AddRecipeFilter(filter_samansha_def, 1)
STRINGS.UI.CRAFTING_FILTERS.SAMANSHA ="自定义过滤器" -- 制作栏中显示的名字
-- 创建一个配方，并将其添加到自定义过滤器中，注意用的是过滤器的name参数值
AddRecipe2("my_custom_recipe", ingredients, tech, config, {"SAMANSHA"})
```

### AddRecipeToFilter

**作用**: 将配方添加到指定的过滤器中。

**参数**

- **`recipe_name`** (string): 要添加的配方名称。
- **`filter_name`** (string): 要添加配方的过滤器名称。

**代码示例**

```lua
-- 将荷叶伞放到「雨具」分类
AddRecipeToFilter("lotus_umbrella","RAIN")
```

### RemoveRecipeFromFilter

**作用**: 从指定的过滤器中移除配方。

**参数**

- **`recipe_name`** (string): 要移除的配方名称。
- **`filter_name`** (string): 要从中移除配方的过滤器名称。


**代码示例**

```lua
-- 将斧头移出「工具」分类
RemoveRecipeFromFilter("axe", "TOOLS")
```


### AddDeconstructRecipe

**作用**: 添加分解配方，不会在任何过滤器下出现，这只是供「分解法杖」或敲碎不可制作的物品而设计的。

**参数**

- **`name`** (string): 要添加配方的prefab名。
- **`return_ingredients`** (string): 返还的材料


**代码示例**

```lua
-- 分解斧头得到一个燧石
AddDeconstructRecipe("axe", {Ingredient("flint", 1)})
```


### AddPrototyperDef

**作用**: 自定义原型机(研究或制作站)在制作菜单中的显示方式。光看这个名字有点难理解，其实原型机就是指科学机器、炼金机器、远古制作台这类东西，过年活动时的神龛也算此类。角色站在旁边时可以制作对应的物品
**参数**

- **`prototyper_prefab`** (string): 用于原型机的prefab名
- **`prototyper_def`** (table)
    - **`icon_atlas`** (string): 图标文档
    - **`icon_image`** (string): 图标图片
    - **`action_str`** (string): 「建造」按钮显示的字符串，比如神龛显示的是「供奉」
    - **`is_crafting_station`** (bool): 是否是制作站，也就是是否只能在停留附近时才能制作对应物品
    - **`filter_text`** (string): 在过滤器上的名称


**代码示例**

```lua
local prototyper_prefab = "moon_altar"
prototyper_def = {
    icon_atlas = "images/xxxx.xml", 
    icon_image = "moon_altar.tex",
    is_crafting_station = true,
    action_str = "生产",
    filter_text = "月之产品“
},

AddPrototyperDef(prototyper_prefab, prototyper_def) 
```


### AddRecipePostInit

**作用**: 修改指定的Recipe

**参数**

- **`recipe`** (string): 要修改的recipe名
- **`fn`** (function): 修改函数，传入参数为这个recipe对象

**代码示例**

```lua
local function fn(recipe)
    -- 修改制作栏描述
    recipe.description = "适合砍树"
end

AddRecipePostInit("axe", fn)
```


### AddRecipePostInitAny

**作用**: 修改任意Recipe

**参数**

- **`fn`** (function): 修改函数，传入参数为recipe对象。这个修改会对所有的recipe生效


**代码示例**

```lua
local function fn(recipe)
    -- 统一修改制作栏描述
    local old_str = recipe.description
    recipe.description = "描述: " ... old_str
end

AddRecipePostInitAny(fn)
```


### 已废弃

以下API已废弃，不推荐在新的Mod制作中使用

* AddRecipe 添加新制作配方
* Recipe 创建配方
* AddRecipeTab 添加制作栏分类

## 其它

### AddUserCommand

**作用**: 向游戏中添加一个自定义用户命令。

**参数**

- **`name`** (string): 命令的名称。
- **`data`** (table): 命令的详细信息，包括以下字段:
    - **`(可选) aliases`** (table): 命令的别名列表。
    - **`(可选) prettyname`** (string): 写在UI上的名称。
    - **`(可选) desc`** (string): 命令的描述。
    - **`permission`** (string): 命令所需的权限（如：COMMAND_PERMISSION.USER）。
    - **`(可选) slash`** (boolean): 是否在命令前加斜杠。
    - **`(可选) usermenu`** (boolean): 是否在用户菜单中显示命令。
    - **`(可选) servermenu`** (boolean): 是否在服务器菜单中显示命令。
    - **`(可选) params`** (table): 命令的参数列表。
    - **`(可选) paramsoptional`** (table): 参数是否可选的布尔值列表。
    - **`(可选) localfn`** (function): 本地函数，当命令被调用时执行。
    - **`(可选) serverfn`** (function): 服务器函数，当命令被调用时执行。
    - **`(可选)vote`** (boolean): 是否允许对此命令进行投票，默认为false。
    - **`(可选)votetimeout`** (number): 投票持续时间（秒）。
    - **`(可选)voteminstartage`** (number): 用户必须在服务器上待至少多少秒才能开始投票。
    - **`(可选)voteminpasscount`** (number): 投票通过所需的最小票数。
    - **`(可选)votecountvisible`** (boolean): 投票结果是否对玩家可见。
    - **`(可选)voteallownotvoted`** (boolean): 是否允许未投票的玩家看到投票结果。
    - **`(可选)voteoptions`** (table): 投票选项
    - **`(可选)votetitlefmt`** (string): 投票标题的格式
    - **`(可选)votenamefmt`** (string): 投票名称的格式
    - **`(可选)votepassedfmt`** (string): 投票通过后的显示信息
    - **`(可选)votecanstartfn`** (function): 检查投票是否可以开始的函数
    - **`(可选)voteresultfn`** (function): 根据投票结果执行相应操作的函数

**代码示例**
```lua
AddUserCommand("hello", {
    aliases = {"hi", "hey"},
    prettyname = "Hello Command",
    desc = "Say hello to the server",
    permission = GLOBAL.COMMAND_PERMISSION.USER,
    slash = true,
    usermenu = false,
    servermenu = false,
    params = {"intro"},
    paramsoptional = {true},
    vote = false,
    serverfn = function(params, caller)
        if params.intro ~= nil then
            GLOBAL.TheNet:Say("Hello, everyone! "..params.intro)
        else
            GLOBAL.TheNet:Say("Hello, everyone!")
        end
    end,
})
```