---
comments: true
---

> 动作节点就是最终执行游戏逻辑的节点，都是叶子节点。除了官方提供的这些，有特殊逻辑需求的，也可以考虑自己自定义。

## 节点列表

{{ read_csv('./data/brain_action.csv') }}


## 节点详情
动作节点就是最终执行游戏逻辑的节点，都是叶子节点。除了官方提供的这些，有特殊逻辑需求的，也可以考虑自己自定义。


### AttackWall
攻击围墙，构造参数只有一个inst，即要实施动作的角色。这个节点会让角色在一系列判断之后尝试攻击围墙。


### AvoidLight
避光，构造参数只有一个inst，即要实施动作的角色。这个节点会让角色尝试躲避光线，并未实际用在任何brain中，但在spier的brain中有require。

### [已废弃]BeargerOffScreen
秋季BOSS熊大在离开屏幕后的行为定义。这个节点是专为熊大设计的，在早期的代码中有使用，目前已经废弃，不再使用，所以也无需了解

### ChaseAndAttack
追逐并攻击。这个节点的参数inst, max_chase_time, give_up_dist, max_attacks, findnewtargetfn, walk，分别为执行动作的对象， 最大追击时间，最大追击距离，最大攻击次数，寻找新目标的函数，是否走路（否则是跑步）。这个节点会让生物在限定条件下，尝试追逐目标并攻击。这个节点默认的执行流程中，并不包含寻找攻击目标的内容，因此，要么定义findnewtargetfn，要么就通过其它的方式来为生物赋予一个攻击目标。在官方代码中，大多数ChaseAndAttack节点都没有findnewtargetfn，是通过其它方式来实现目标设置的。最常见的就是保持默认设置：生物被攻击后，会把攻击者设置为攻击目标。

### ChaseAndAttackAndAvoid
追逐、攻击并躲避。这个节点的参数inst, findavoidanceobjectfn, avoid_dist, max_chase_time, give_up_dist, max_attacks, findnewtargetfn, walk，可以看到基本和ChaseAndAttack类似，区别在于增加了两个参数findavoidanceobjectfn, avoid_dist。也就是这个节点会在追逐攻击的同时，试图躲避某些目标。比如猪人战士，在攻击时回调整自己的攻击位置，避免过于靠近猪王。findavoidanceobjectfn, avoid_dist分别表示找到躲避目标的函数和躲避的距离。


### ChaseAndRam
追逐并冲撞。节点参数inst, max_chase_time, give_up_dist, max_charge_dist, max_attacks，分别是生物自身，最大追逐时间，最大追逐距离，最大冲撞距离，最大攻击次数。如果目标与自身的距离超过最大冲撞距离，就无法实行冲撞。这个节点用于秋季BOSS，地下的犀牛BOSS这些具有冲撞能力的BOSS上。


### ControlMinions
控制子体。这个节点仅在食人花上使用，控制食人花的子体。它会在子体的控制范围内进行一系列的行动，比如捡起地上的东西或者收获农场的食物等。


### DoAction
通用的动作节点。节点参数inst, getactionfn, name, run, timeout，分别是生物自身，获取动作的函数（返回值必须是一个动作），是否在执行动作时跑起来，超时时间。这个节点可以通过设置getactionfn来让生物在满足某些条件的情况下执行特定动作，最常见的就是在某些条件下执行回家的动作。


### FaceEntity
面向某个实体。节点参数inst, getfn, keepfn, timeout，分别是生物自身，搜寻目标的函数，保留目标的判断函数，超时时间。这个节点可以让生物面向特定的对象。如果你仔细观察跟随你的生物，你会发现它们在静止时总是面向你的，这就是FaceEntity在发挥作用。

### FindClosest
寻找最近的单位。节点参数inst, see_dist, safe_dist, tags, exclude_tags, one_of_tags，分别是生物自身，搜寻距离，安全距离（当搜寻到目标后，会尝试移动到安全距离内），搜寻标签（搜寻单位必须包含全部的标签），排除标签（搜寻单位不能其中的一个标签），其中之一标签（如果这个参数不为空，则可以无视前两个参数，只要单位具有这个参数中的任意一个标签就算是匹配了）。这个节点主要用于寻找符合标签要求的目标，并且自动移动到目标附近。

### FindFlower
从名字就可以看出来，这个节点的作用就是寻找花，用于蜜蜂和蝴蝶。


### FindLight
寻找光。节点参数是inst, see_dist, safe_dist，分别为生物自身，搜寻距离，安全距离。参数具体含义同FindClosest。这个节点就是为了寻找光源，用于猪人。


### Follow 
跟随。这个节点会令生物跟随目标移动。

节点参数如下

* inst: 生物自身
* target: 搜寻跟随目标的函数
* min_dist: 最小距离（目标与生物之间的距离低于此距离，生物会远离目标，直到距离大于target_dist）
* target_dist: 目标距离（最合适的距离）
* max_dist: 最大距离（目标与生物之间的距离高于此距离，生物会靠近目标，直到距离小于target_dist）
* canrun: 是否可以奔跑，决定移动时是走还是跑



### Leash
绑定。这个节点会令生物绑定在它的领地上，不会到处乱跑。节点参数inst, homelocation, max_dist, inner_return_dist, running，分别为生物自身，领地位置（可以是具体坐标或一个函数），最大距离（超过此距离时，生物会停止走动，结束本轮的检测），返回距离（超过此距离时，生物会试图返回领地位置），返回时是否奔跑。这个节点实际生效的场景是这样的，生物追逐玩家跑出的领地的最大距离后，会先停在原地，然后等到下一次检测时，返回原本位置。
如果在游戏中仔细观察，就会注意到很多有领地的生物比如龙蝇，在追得太远时都会先停一下才返回。



### LeashAndAvoid
绑定并躲避。和ChaseAndAttackAndAvoid类似，参数设置也相似，就是在Leash的基础上增加了躲避，同样也仅仅用于猪人守卫，不再赘述。


### Panic
慌乱。这个动作执行时，生物会随机乱窜。


### PanicAndAvoid
慌乱并躲避，和ChaseAndAttackAndAvoid类似，同样也仅仅用于猪人守卫，不再赘述。


### RunAway
逃离。这个节点可以让生物远离特定目标（这里称为hunter），还可以回家。常见于兔子等生物。
节点参数如下
* inst: 生物自身
* hunterparams: hunter的相关参数如搜寻函数、标签等
* see_dist: 搜寻距离，当发现hunter小于此距离时，会逃跑
* safe_dist: 安全距离，开始逃跑后，当hunter大于此时，就会停下
* fn: 额外的判定fn，如果这个参数不为空，需要fn返回true才会逃跑
* runhome: 是否跑回家。
* fix_overhang: 如果参数为true，则会检测生物的位置，如果在海上会被移动到陆地


### [已废弃]StalkerChaseAndAttack
一个过期的节点，官方推荐使用ChaseAndAttackAndAvoid替代


### StandAndAttack
站在原地攻击。部分没有移动能力的生物，会使用这个节点而不是ChaseAndAttack。参数inst, findnewtargetfn，分别是生物自身，和寻找新目标的函数。第二个参数在官方的代码中并没有用到过。



### StandStill
站在原地不动。参数inst, startfn, keepfn，分别是生物自身，开始这个动作的判定函数，保持这个动作的判定函数。这个动作节点很简单，就是让生物保持站在原地不动，如果startfn和keepfn为空，则不需要任何条件，只要行为树访问到这里，就会执行这个动作，保持不动。



### UseShield
使用护盾防御。石虾在被攻击时经常会使用这个动作。参数inst, damageforshield, shieldtime, hidefromprojectiles, hidewhenscared，分别为生物自身，当自身收到伤害多少时开启护盾防御，防御持续时间，是否防御远程攻击，在受惊时是否防御。
这个节点制定了进入防御状态和退出防御状态的逻辑，实质上是触发了两个事件，分别是entershield和exitshield。如果你希望构建一种具备防御姿势的生物，可以考虑使用这个节点，并且为事件entershield和exitshield设置相应的回调函数（在官方代码中，是设置了相应的SG的EventHandler)


### Wander
漫游。生物会在自己的家附近游荡。

参数说明

* inst: 生物自身
* homelocation: 家的位置，可以是具体的位置或者一个函数
* max_dist: 最大游荡距离，生物与家的距离超过此距离时，就会往家里走
* times: 这是一个times，分别有minwalktime，randwalktime，minwaittime，randwaittime四个子key，用于设置漫游中间的间隔时间，不需要填完全部参数，都不填也可以，都有默认设置的相应时间。
* getdirectionFn: 获取返回的行走角度的函数，没有的时候就随机取一个角度
* setdirectionFn: 修正返回行走角度的函数，没有的话就略过。
* checkpointFn: 搜寻路径用的检测函数，有时用于绕过围墙。

getdirectionFn，setdirectionFn，checkpointFn这三个参数在实际的使用中较少用到，一般情况下可以忽略。

