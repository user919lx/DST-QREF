Component 组件在持续更新中，可参见完整列表。




## 常用

* 移动: [locomotor](components/locomotor.md)
* 劳作: workable, pickable, finiteuses
* 种植: growable
* 食用: edible, eater, perishable, hunger, sanity
* 战斗: combat, weapon,health , aoetargeting（是否可以做出闪避的效果？）
* 口袋: inventory, inventoryitem, stackable, container, equippable, inspectable
* 掉落: lootdropper
* 篝火: fueled, fuel
* 交易: trader, 
* 状态: burnable, propagator, freezable
* 检查: inspectable
* 作祟: hauntable
* 动物行为
    * 跟随: follower, leader
    * 睡觉: sleeper
    * 产出,生成: spawner, periodicspawner（拉屎）, childspawner
    * 骑: rider
    * 回家: knownlocations, homeseeker
    * 轨迹: entitytracker（大象脚印）
* 其它
    * playercontroller
    * timer



## 完整列表

这些组件的含义是通过AI识别名称语义写成的，仅仅是提供一个可参考的说明。**有可能有错误，还需要人工校验**。

is_verified=Yes的组件表示已经过人工验证，verified_by指出被谁验证过了

{{ read_csv('./data/components.csv') }}





