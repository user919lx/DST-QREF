装饰或组合节点都有子节点。本身不负责具体的游戏交互动作，而是负责处理子节点访问前后的相关逻辑，比如访问前的判断，访问后的状态修饰等。


## 节点列表

{{ read_csv('./data/brain_dec.csv') }}


## 节点详情

### DecoratorNode等

DecoratorNode, NotDecorator, FailIfRunningDecorator, FailIfSuccessDecorator
这几个节点都是一脉相承的，就放在一起说明了。
DecoratorNode 正如其名，是个修饰节点，无任何效果，也未直接使用于任何brain中。唯一的用途是作为几个特殊节点的父类，分别是NotDecorator、FailIfRunningDecorator、FailIfSuccessDecorator。这四个节点，都只有一个构建参数child，即子节点，也就是说这几个节点都拥有唯一一个子节点。它们之间的区别在于会根据子节点的状态来调整本身的状态。

* DecoratorNode:无论子节点如何，状态均为FAILED
* NotDecorator：子节点为SUCCESS，自身则为FAILED；子节点为FAILED，自身则为SUCCESS。其它情况子节点状态等同自身状态。
* FailIfRunningDecorator：子节点为RUNNING，自身则为FAILED。其它情况子节点状态等同自身状态。
* FailIfSuccessDecorator：子节点为SUCCESS，自身则为FAILED。其它情况子节点状态等同自身状态。

### ConditionNode
条件节点，构造函数中附带一个function参数，称之为检测函数，每当访问该节点时，会执行检测函数，如果结果为true，则节点状态为SUCCESS，否则为FAILED。这个节点直接使用的场景比较少，通常来说，主要是作为WhileNode或IfNode的一部分，在大部分场景中都是直接使用WhileNode或IfNode。适合使用这个节点的场景是，判定条件后续的动作节点有多个，这种情况下无法使用WhileNode或IfNode，因为它们只有一个node。

### ConditionWaitNode
与ConditionNode很相似，唯一的区别在于，检测函数返回false时，节点状态不是FAILED而是RUNNING。这个节点也仅仅是做了定义，并没有在任何brain中被使用。

### ActionNode
这个就是一般意义上的动作节点。构建参数为action, name，分别为动作函数和节点名称。在访问该节点时，会执行动作函数，无论执行结果如何，最后节点的状态都是SUCCESS。这个节点适合执行一些不需要记录状态来调整后续操作的情况。比如小鸟飞走，这个动作只需要执行后就可以不再理会，就很适合用ActionNode来承载。

### WaitNode
等待节点，构造参数为time，即等待时间。当所经过的时间小于设置的等待时间时，节点状态为RUNNING，否则为SUCCESS。也就是只要访问到这个节点，就需要等待一段时间才可以进行下一步的操作。

### SequenceNode
序列节点，主要用途就是按顺序遍历访问子节点。当节点状态不是RUNING时，会从第一个子节点开始遍历，否则就从上一次执行时的RUNING状态的子节点开始。按顺序访问子节点，当子节点状态为RUNING或FAILED时，中断本次运行，节点的状态等同该子节点的状态。如果子节点的状态为SUCCESS，则继续访问下一个子节点。如果整个遍历过程没有中断地全部完成了，则节点状态为SUCCESS。这一节点适合用于执行需要持续执行的动作，它能够记录上一次正在执行的动作，从而保证在多个间隔中保持执行动作的连续性，让一个序列里的所有动作都能按顺序执行完成。
最常见的就是和WaitNode搭配使用，可以在执行某个动作前强制等待一会儿。比如小偷克劳斯会在等待一段时间后自动回复血量。

### SelectorNode
选择节点。访问的步骤是和SequenceNode一样的，只是把其中的FAILED和SUCCESS相关的部分对调了。这个节点在被访问到时，如果是RUNING状态，则继续访问上一次访问的RUNING子节点，否则，重新开始按顺序遍历访问全部子节点。直到其中一个子节点的状态为RUNING或SUCCESS，该子节点的状态就等同该节点的状态。如果所有子节点状态都不是RUNING或SUCCESS，则该节点的状态为FAILED。也就是说，这个节点会选择遍历到的第一个状态不是FAILED的节点。举例来说，可以做到这样的效果

用一个实际例子来说明会更容易理解
![SelectorNode](/images/SelectorNode.png)

上图是一个简单的攻击选择器。首先尝试原地攻击，如果目标距离超过攻击范围，则FAILED。然后尝试追逐目标，缩短距离，如果成功缩短距离就SUCCESS，然后重新执行原地攻击。如果检测到有墙体，无法快速接近目标，则FAILED。接着就会访问破墙节点，尝试破坏墙体，成功后重新遍历操作。

这个节点可以处理一些复杂的分支选择，不过目前还未在官方的brain中应用过。

### LoopNode
循环节点。和SequenceNode的执行流程是相似的，区别在于增加了循环。节点会一直按SequenceNode的流程访问子节点。当子节点全部访问完成后会重置，重新开始，并且令循环次数+1。
结束循环有两种方式：
* 访问流程中有子节点返回了FAILED
* 循环次数达到了预先设置的最大循环次数
如果没有设置循环次数，又没有子节点返回FAILED，则这个循环流程会一直持续下去。

一个常见的例子是猪人砍树。如果你仔细观察，会发现猪人每次砍树的时候都会碎碎念，说一句话，砍一棵树。

### RandomNode
从名字来看，是一个随机选择的节点。不过代码看起来有点问题，而且也没有应用于任何brain，忽略这个节点。

### PriorityNode
这个节点通常是作为整个brain的根节点来使用的。其访问的流程有多重判断，比较复杂，了解其流程也无助于理解使用，因此就不做细致的讲解了。只需要知道，第一个参数为子节点的table，第二个参数为检测间隔即可，第三个参数常常是不填的，可以忽略。除了作为根节点使用外。也可以作为序列节点使用，类似SequenceNode，额外增加的功能是可以设置独立的检测时间间隔，但不可以高于根节点的间隔，否则根节点会先刷新。

### ParallelNode和ParallelNodeAny
两个节点很相似，放在一起说明。ParallelNode正如其名，平行节点，会平行地访问所有的子节点，在每一轮间隔中，都会访问所有状态不为SUCCESS的节点。当其中一个子节点状态为FAILED时，自身状态变为FAILED。当所有的子节点状态都为SUCCESS时，自身状态变为SUCCESS。其它的情况下，自身的状态均为RUNNING。ParallelNodeAny是ParallelNode的子类，FAILED的条件是一样的，区别在于SUCCESS。只要有一个子节点为SUCCESS且没有子节点为FAILED，自身的状态就会变为SUCCESS。

### EventNode
事件节点。构造参数有四个，inst, event, child, priority。

* inst：要监听事件的目标实体
* event：要监听的事件名
* child：事件触发时要执行访问的子节点
* priority：执行优先级，当有多个EventNode时，会根据优先级来确认先执行哪一个。

这个节点会为inst设置事件的监听回调，当inst触发事件时，执行访问child。常见的用法是，inst自身触发gohome事件，然后child设置的动作为回家。



### WhileNode和IfNode
这两个节点很相似，就放在一起说明。首先，这两个所谓的节点并不是真正意义上的节点，实际上是一个构造函数，会返回一个组合好的节点。WhileNode会返回一个ParallelNode，分别含有两个子节点，分别为ConditionNode和要访问的动作节点。IfNode也很相似，区别在于ParallelNode变成了SequenceNode。两个构造函数的参数都是cond,name,node，分别为判定函数，节点名称和动作子节点，这个动作子节点会在判定函数返回true时被访问。两者的区别在于，WhileNode会在每个检测周期开始时，重新进行判定，如果判定不为true，不管子节点当前的的status如何，都会强制FAILED。而IfNode则是一旦判定为true，后续的每个检测周期都会略过判定直接访问子节点，直到子节点的状态变为SUCCESS或FAILED。
简单地说，两者使用的区别就在于**你是否希望子节点的执行能被打断**。如果你希望不断检测状态来决定是否执行下一步动作，就使用WhileNode。如果你希望判定成功一次后就一直执行下一步动作直到完成或失败，就选择IfNode。两者在游戏中都有大量地使用，让我们来举例说明。比如，生物被作祟或者着火之后会执行Panic动作，是很常见的设置。在这个场景中，我们肯定希望当作祟或者着火状态结束后，生物就不再Panic，这时候就应该使用WhileNode。而另外一个场景，鸟在看到人靠近的时候会直接飞走，在这个场景中，人只要靠近了，不管之后是否离开了检测范围，一旦触发，鸟就会不受打断地直接飞走。在这个场景中，就应该使用IfNode。

以下代码均出自`brains/birdbrain.lua`，也就是鸟的AI设置，分别使用了WhileNode和IfNode，可以看出它们的区别。
```lua
-- 一旦作祟状态结束，Panic就会停止
WhileNode( function() return self.inst.components.hauntable ~= nil and self.inst.components.hauntable.panic end, "PanicHaunted", Panic(self.inst)),
-- 一旦鸟感知到了危险，就会不受打断地飞走。
IfNode(function() return ShouldFlyAway(self.inst) end, "Threat Near",
    ActionNode(function() return FlyAway(self.inst) end)),
```


### LatchNode
锁节点。从代码上看，应该是要锁住brain一段时间。没有想到合适的使用场景，官方代码的brain中也没有用到，略过。

### ChattyNode
这个节点虽然算是修饰节点，但是单独定义在一个文件里的，位于`behaviours/chattynode.lua`。其代码包含游戏逻辑上的互动，在某种意义上也可以算是一个行为节点，但它带有唯一子节点，所以还是归类为修饰节点。

这个节点的作用是，当子节点处于RUNING状态时，会令角色说话，要说的内容可以通过构建函数中的`chatlines`参数传入。传字符串时，对应的是全局变量`STRINGS`下的一个key，传table时，则需要其中每个元素都填写完整的说话内容。每次说话后，会有一个随机时长的短暂沉默时间。



### MinPeriod
与ChattyNode类似，单独定义在一个文件里，位于`behaviours/minperiod.lua`。它的作用是，可以周期性地执行访问子节点。常见的使用场景是，为生物执行某中动作设置一个CD，避免其连续执行。比如小偷坎普斯，偷东西是有CD的，不会连续偷取。