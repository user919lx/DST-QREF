---
comments: true
---

> 系统组件，指的是代码不可见的，由底层C++编写后仅暴露借口的各种重要组件


### AnimState
负责控制角色的外观和动画，人物执行不同的动作会播放不同的动画，佩戴某些装备会改变外观，以及改变视觉上的大小，甚至计量条的变化等。凡是和物体形态变化有关的，都由AnimState来操作实现。

|fn|用途|传入参数|返回值|
| :----: | :----: | :----: | :----: |
|AddOverrideBuild|添加覆盖Build，比如给鸟笼添加鸟的build。对于骑乘类，这个函数很重要|附加的build|无|
|AnimDone|判断动画是否播完|无|bool 是否播完|
|AssignItemSkins|为物品设置皮肤|user_id 用户id，剩余参数不定，分别是各个部位的皮肤名|无|
|BuildHasSymbol||||
|ClearAllOverrideSymbols||||
|ClearBloomEffectHandle||||
|ClearOverrideBuild||||
|ClearOverrideSymbol|清除指定标记点的覆盖|inst_symbol 要清除覆盖的标记点|无|
|ClearSymbolExchanges||||
|CompareSymbolBuilds||||
|FastForward||||
|GetAddColour||||
|GetBuild||||
|GetCurrentAnimationFrame||||
|GetCurrentAnimationLength|获取当前动画的长度|无|length 动画长度|
|GetCurrentAnimationTime|获取当前动画停留在第几秒|无|time 停留位置|
|GetCurrentFacing||||
|GetInheritsSortKey||||
|GetMultColour|获取角色的r,g,b,a|无|r,g,b,a 同SetMultColour的输入参数|
|GetSkinBuild||||
|GetSortOrder||||
|GetSymbolPosition||||
|Hide|隐藏某个部分，和Show搭配使用|part 部分|无|
|HideSymbol||||
|IsCurrentAnimation|检测当前播放的动画是否为指定的动画|anim 动画名|无|
|OverrideItemSkinSymbol||||
|OverrideMultColour||||
|OverrideShade||||
|OverrideSkinSymbol||||
|OverrideSymbol|覆盖某个标记点，常见于装备武器后，手上就出现了一把武器。|inst_symbol 要覆盖的标记点 ;swap_build 用于替换的build ;swap_symbol 用于替换的build上的标记点|无|
|Pause||||
|PlayAnimation|播放指定名称的动画，会立刻中断当前动画的播放|anim 动画名; loop 是否重复，可省略，默认值是fasle|无|
|PushAnimation|将指定动画推送到播放序列中，当前动画播放完后会接着播放这个动画，常见于要通过一组动画来表现人物的场景|anim 动画名; loop 是否重复，可省略，默认值是fasle|无|
|Resume||||
|SetAddColour|设置附加颜色|r,g,b,a四个参数，分别对应红，绿，蓝的颜色值以及透明度。取值均在[0,1]之间。对于透明度，取0时就是完全透明。|无|
|SetBank|设置指定的动画组。玩家站在地上和骑在牛上，使用的是两套不同的动画，就是通过设置不同的Bank来实现的|bank 动画组名|无|
|SetBankAndPlayAnimation||||
|SetBloomEffectHandle|设置Bloom效果的处理器|path 处理器路径|无|
|SetBuild|设置指定的外观。比如兔子有夏、冬两种形态，就是通过设置不同的Build来实现的|build 外观名|无|
|SetClientSideBuildOverrideFlag||||
|SetClientsideBuildOverride||||
|SetDeltaTimeMultiplier||||
|SetDepthBias||||
|SetDepthTestEnabled||||
|SetDepthWriteEnabled||||
|SetErosionParams||||
|SetFinalOffset|不确定，根据函数名猜测，是设置动画的帧偏移量。|offset 动画偏移量，可以设置为负数。|无|
|SetFloatParams||||
|SetHaunted||||
|SetHighlightColour||||
|SetInheritsSortKey||||
|SetLayer|设置图层，图层是有固定的摆放顺序的，比如土地是最下一层，然后农场是中间层，农场里的作物是最上层。在构建一些多层结构的东西时，都需要设置图层。|layer 图层变量，这里使用定义于constants.lua中的全局变量，LAYER_BACKGROUND/LAYER_WORLD/LAYER_WORLD_BACKGROUND/LAYER_WORLD_CEILING/LAYER_FRONTEND|无|
|SetLightOverride||||
|SetManualBB||||
|SetMultColour|设置角色的r,g,b,a，也就是三个颜色+透明度。可以通过这个函数来让角色变得透明|r,g,b,a四个参数，分别对应红，绿，蓝的颜色值以及透明度。取值均在[0,1]之间。对于透明度，取0时就是完全透明。|无|
|SetMultiSymbolExchange||||
|SetOceanBlendParams||||
|SetOrientation|设置刚体轴方向。不同的轴方向会影响看到的视觉效果，比如池塘看起来是贴着地面的，就是因为设置了这一参数为ANIM_ORIENTATION.OnGround|direction 方向，这里使用定义于constants.lua中的全局变量，ANIM_ORIENTATION下的各个值|无|
|SetPercent|设置动画百分比，对于一些通过动画帧来表现数值的物品很有用。比如雨量计，实际上是设置了一个动画，从0到100%，然后根据实际数值设置相应的百分比|anim 动画名; percent 百分比|无|
|SetRayTestOnBB||||
|SetScale|设置缩放比例|length_scale 长度缩放;width_scale 宽度缩放 。取值填小数，如果是负数，则是相应的方向颠倒。|无|
|SetSkin||||
|SetSortOrder|设置排序优先级，常与SetLayer配套使用，当有多个物品重叠时，优先级高的排在前面。|priority 优先级，整数|无|
|SetSortWorldOffset||||
|SetSymbolExchange||||
|SetTime|设置动画停留在第几秒|time 停留位置|无|
|Show|展示某个部分，通常和Hide搭配使用。比如，装备武器时，会隐藏ARM_normal，显示ARM_carry，人物的手就发生了变化|part 部分|无|
|ShowSymbol||||

### DynamicShadow
负责管理物体的影子。每个物体的影子都各不相同，甚至同一个角色，在不同的装备下也有不同的效果。大家可以试试装备不同的雨伞观察一下影子的变化。

|fn|用途|传入参数|返回值|
| :----: | :----: | :----: | :----: |
|SetSize|设置影子大小|length_scale 长度缩放;width_scale 宽度缩放 。取值填小数。|无|
|Enable|设置是否有影子|取值 ture/false|无|

### EnvelopeManager
信封管理？不太确定

|fn|用途|传入参数|返回值|
| :----: | :----: | :----: | :----: |
|AddColourEnvelope||||
|AddFloatEnvelope||||
|AddVector2Envelope||||

### Follower
在component中也有一个同名的follower，但这个系统组件的follower是更底层的，常用于一些特效跟随的处理。而component的follower更侧重于一个生物跟随另一个生物。

|fn|用途|传入参数|返回值|
| :----: | :----: | :----: | :----: |
|FollowSymbol|跟随标记，常用于特效跟随于某个物体。|target_guid 跟随目标的guid;symbol 跟随的标记点; x,y,z 在三个方向上的偏移量|无|
|SetOffset||||

### FontManager
字体管理

|fn|用途|传入参数|返回值|
| :----: | :----: | :----: | :----: |
|RegisterFont||||

### Label
标签

|fn|用途|传入参数|返回值|
| :----: | :----: | :----: | :----: |
|Enable||||
|SetColour||||
|SetFont||||
|SetFontSize||||
|SetText||||
|SetUIOffset||||
|SetWorldOffset||||

### Light
负责管理光源，可以为一个物体添加光源并调整设置相关参数如发光半径、强度、衰减度等。

|fn|用途|传入参数|返回值|
| :----: | :----: | :----: | :----: |
|Enable|设置光源是否可用|取值 ture/false|无|
|EnableClientModulation|待定，未确认用途|||
|GetCalculatedRadius|获取光源半径|无|radius 半径距离|
|GetColour|获取光源颜色|无|r,g,b 分别对应红，绿，蓝的颜色，取值[0,1]|
|GetDisableOnSceneRemoval||||
|GetFalloff||||
|GetIntensity||||
|GetRadius||||
|IsEnabled|判断光源是否可用|无|取值 ture/false|
|SetColour|设置光源颜色|r,g,b 分别对应红，绿，蓝的颜色，取值[0,1]|无|
|SetDisableOnSceneRemoval||||
|SetFalloff|设置衰减强度|falloff 衰减强度,取值[0,1]|无|
|SetIntensity|设置光源亮度|intensity 亮度,取值[0,1]|无|
|SetRadius|设置光源半径。在很多涉及光的计算中都会用到光源半径，比如作物生长需要光源，这个光源是有距离要求的，太远的就不算。|radius 半径距离|无|

### LightWatcher
光照监视器，观测光照的情况。在游戏中，很多动植物的活动都和光照有关。例如蜘蛛一般情况下只在晚上和黑暗环境下活动，猪人则在白天或者明亮的环境下活动。如何判断黑暗和明亮，就是光照监视器来做的。

|fn|用途|传入参数|返回值|
| :----: | :----: | :----: | :----: |
|GetLightAngle|获取光照角度|无|angle 光照角度|
|GetLightValue|获取光照值|无|light_val 光照值|
|GetTimeInDark||||
|GetTimeInLight|获取光照时长|无|time 光照时长|
|IsInLight|判断是否在明亮环境|无|取值 ture/false|
|SetDarkThresh|设置黑暗阈值|thresh 阈值，取值[0,1]|无|
|SetLightThresh|设置明亮阈值|thresh 阈值，取值[0,1]|无|

### Map
地图

|fn|用途|传入参数|返回值|
| :----: | :----: | :----: | :----: |
|SetMinimapOceanEdgeColor0||||
|IsVisualGroundAtPointDebug||||
|GetTileCenterPoint||||
|SetMinimapOceanEdgeParams1||||
|SetOceanTextureBlendAmount||||
|SetMinimapOceanEdgeNoiseParams||||
|GetIslandAtPoint||||
|InternalIsPointOnWater||||
|GetNodeIdAtPoint||||
|SetWaterfallFadeParameters||||
|GetRandomPointsForSite||||
|GetStringEncode||||
|SetOverlayColor2||||
|SetImpassableType||||
|CalcPercentLandTilesAtPoint||||
|SetOceanTextureBlurParameters||||
|SetClearColor||||
|Finalize||||
|IsVisualGroundAtPoint||||
|CanTerraformAtPoint||||
|SetNavSize||||
|IsDeployPointClear||||
|SetSize||||
|CanDeployMastAtPoint||||
|FindRandomPointInOcean||||
|GenerateBlendedMap||||
|GetTileAtPoint||||
|GetWorldSize||||
|GetNearestPointOnWater||||
|SetOverlayColor1||||
|ResetVisited||||
|SetOceanNoiseParameters0||||
|SetMinimapOceanEdgeFadeParams||||
|SetWaterfallNoiseParameters0||||
|CalcPercentOceanTilesAtPoint||||
|GetPlatformAtPoint||||
|SetUndergroundRenderLayer||||
|SetOverlayLerp||||
|SetMinimapOceanEdgeShadowParams||||
|AddRenderLayer||||
|IsPassableAtPointWithPlatformRadiusBias||||
|IsSurroundedByWater||||
|CanDeployRecipeAtPoint||||
|IsOceanAtPoint||||
|RegisterGroundTargetBlocker||||
|GetNavStringEncode||||
|CanDeployAtPointInWater||||
|Replace||||
|IsFarmableSoilAtPoint||||
|IsOceanTileAtPoint||||
|CanDeployAtPoint||||
|IsGroundTargetBlocked||||
|GetEntitiesOnTileAtPoint||||
|CanTillSoilAtPoint||||
|CanDeployWallAtPoint||||
|SetOceanEnabled||||
|RepopulateNodeIdTileMap||||
|CanDeployPlantAtPoint||||
|SetPhysicsWallDistance||||
|CanPlantAtPoint||||
|IsAboveGroundAtPoint||||
|TileVisited||||
|CanPlowAtPoint||||
|IsValidTileAtPoint||||
|CollapseSoilAtPoint||||
|CanPlaceTurfAtPoint||||
|SetMinimapOceanEdgeShadowColor||||
|RegisterDeployExtraSpacing||||
|RegisterTerraformExtraSpacing||||
|IsPassableAtPoint||||
|VisitTile||||
|RetrofitNavGrid||||
|SetMinimapOceanTextureBlurParameters||||
|SetOceanNoiseParameters2||||
|GetNodeIdTileMapStringEncode||||
|SetMinimapOceanEdgeParams0||||
|SetMinimapOceanEdgeColor1||||
|SetUndergroundFadeHeight||||
|GetSize||||
|SetOverlayColor0||||
|SetOceanNoiseParameters1||||
|SetTransparentOcean||||
|Fill||||
|GetTileCoordsAtPoint||||
|SetNodeIdTileMapFromString||||
|SetTileNodeId||||
|GetTileXYAtPoint||||
|GetTile||||
|SetNavFromString||||
|SetWaterfallNoiseParameters1||||
|SetMinimapOceanMaskBlurParameters||||
|SetTile||||
|SetOverlayTexture||||
|FindNodeAtPoint||||
|SetFromString||||
|NodeAtPointHasTag||||
|RebuildLayer||||
|IsPointNearHole||||
|CanPlacePrefabFilteredAtPoint||||

### MapExplorer
地图探索

|fn|用途|传入参数|返回值|
| :----: | :----: | :----: | :----: |
|ActivateLocalMiniMap||||
|EnableUpdate||||
|LearnAllMaps||||
|LearnRecordedMap||||
|RecordAllMaps||||
|RecordMap||||
|RevealArea||||

### MapLayerManager
地图涂层管理

|fn|用途|传入参数|返回值|
| :----: | :----: | :----: | :----: |
|CreateRenderLayer||||
|ReleaseRenderLayer||||
|SetMinimapColor||||
|SetPrimaryColor||||
|SetSampleStyle||||
|SetSecondaryColor||||
|SetSecondaryColorDusk||||

### MiniMap
小地图

|fn|用途|传入参数|返回值|
| :----: | :----: | :----: | :----: |
|AddAtlas||||
|AddRenderLayer||||
|ClearRevealedAreas||||
|ContinuouslyClearRevealedAreas||||
|DrawForgottenFogOfWar||||
|EnableFogOfWar||||
|IsVisible||||
|RebuildLayer||||
|SetEffects||||
|ShowArea||||
|ToggleVisibility||||

### MiniMapEntity
负责管理小地图的图标

|fn|用途|传入参数|返回值|
| :----: | :----: | :----: | :----: |
|CopyIcon||||
|SetCanUseCache|待定，设置可使用缓存|是否可用,取值ture/false|无|
|SetDrawOverFogOfWar|待定，设置可无视迷雾显示图标|是否可用,取值ture/false|无|
|SetEnabled|设置小地图图标是否可用|是否可用,取值ture/false|无|
|SetIcon|设置小地图图标|image 图标文件名|无|
|SetIsFogRevealer|待定，设置可以显示迷雾|是否可用,取值ture/false|无|
|SetIsProxy||||
|SetPriority|设置优先级，高优先级可以显示在更上面|priority 优先级，整数|无|
|SetRestriction||||

### Network
负责管理网络

|fn|用途|传入参数|返回值|
| :----: | :----: | :----: | :----: |
|GetNetworkID||||
|GetUserID||||
|GetUserFlags||||
|GetPlayerColour||||
|IsPlayingWithFriends||||
|IsBorrowed||||
|GetClientName||||
|AddUserFlag|待定，无法判断|||
|SetConsecutiveMatch||||
|IsConsecutiveMatch||||
|SetPlayerEquip||||
|SetPlayerSkin||||
|SetPlayerAge||||
|SetClassifiedTarget|待定，无法判断|||
|RemoveUserFlag|待定，无法判断|||
|IsServerAdmin||||
|GetPlayerAge||||

### Pathfinder
寻路

|fn|用途|传入参数|返回值|
| :----: | :----: | :----: | :----: |
|AddWall||||
|GetPathTileIndexFromPoint||||
|GetSearchResult||||
|GetSearchStatus||||
|HasWall||||
|IsClear||||
|KillSearch||||
|RemoveWall||||
|SubmitSearch||||

### Physics
管理物体的物理运动相关的内容，包括物理参数，移动，碰撞等。
通常而言，不建议自己配置物理运动相关的参数，错误参数可能造成一些诡异的物理效果。最好使用官方给出的预设函数来一键配置，位于`standardcomponents`

|fn|用途|传入参数|返回值|
| :----: | :----: | :----: | :----: |
|CheckGridOffset||||
|ClearCollidesWith||||
|ClearCollisionMask||||
|ClearLocalCollisionMask||||
|ClearMotorVelOverride||||
|ClearTransformationHistory||||
|CollidesWith|设置可以触发碰撞的类型|collision_type 碰撞类型，这里使用定义在constants.lua下的全局变量，COLLISION表下的内容|无|
|ConstrainTo||||
|GeoProbe||||
|GetCollisionGroup||||
|GetCollisionMask||||
|GetHeight||||
|GetMass||||
|GetMotorSpeed||||
|GetMotorVel||||
|GetRadius||||
|GetVelocity||||
|IsActive||||
|IsPassable||||
|SetActive||||
|SetCapsule|待定|||
|SetCollides||||
|SetCollisionCallback||||
|SetCollisionGroup|设置自身的碰撞类型|collision_type 碰撞类型，这里使用定义在constants.lua下的全局变量，COLLISION表下的内容|无|
|SetCollisionMask||||
|SetCylinder||||
|SetDamping||||
|SetDontRemoveOnSleep||||
|SetFriction||||
|SetLocalCollisionMask||||
|SetMass|设置物体的质量，会影响到一些物理效果|mass 物体质量|无|
|SetMotorVel|为物体设置一个移动速度|x,y,z 物体在三个方向上的移动速度,其中,x是物体在地面上朝向方向，y是垂直地面向上的方向，z是与x在地面上正交的方向|无|
|SetMotorVelOverride||||
|SetRestitution||||
|SetRigidBodyEnabled||||
|SetSphere||||
|SetTriangleMesh||||
|SetVel|待定，需要实测代码确定和motorvel的不同|x,y,z 物体在三个方向上的移动速度,其中,x是物体在地面上朝向方向，y是垂直地面向上的方向，z是与x在地面上正交的方向|无|
|Stop||||
|TEMPHACK_DisableSleepDeactivation||||
|Teleport|瞬移到指定位置|x,y,z 要传送位置的三维坐标|无|
|TeleportRespectingInterpolation||||

### PostProcessor
色彩特效相关

|fn|用途|传入参数|返回值|
| :----: | :----: | :----: | :----: |
|SetColourCubeData||||
|SetColourCubeLerp||||
|SetColourModifier||||
|SetDistortionFactor||||
|SetDistortionRadii||||
|SetEffectTime||||
|SetLunacyEnabled||||
|SetOverlayBlend||||
|SetOverlayTex||||
|SetPostProcessingEnabled||||

### RoadManager
道路管理

|fn|用途|传入参数|返回值|
| :----: | :----: | :----: | :----: |
|AddControlPoint||||
|AddSmoothedControlPoint||||
|BeginRoad||||
|GenerateQuadTree||||
|GenerateVB||||
|IsOnRoad||||
|SetStripEffect||||
|SetStripTextures||||
|SetStripUVAnimStep||||
|SetStripWrapMode||||

### SoundEmitter
负责声音控制，播放物体本身拥有的音效资源。

|fn|用途|传入参数|返回值|
| :----: | :----: | :----: | :----: |
|GetEntity||||
|KillAllSounds||||
|KillSound|停止播放指定音效|sound 音效名|无|
|OverrideVolumeMultiplier||||
|PlaySound|播放指定音效|path 音效文件路径|无|
|PlaySoundWithParams|带着参数播放音效|path 音效文件路径,param_tbl 参数表，形如{param_a=xxx,param_b=xxx}|无|
|PlayingSound|判断是否在播放指定音效|sound 音效名|is_playing 是否在播放该音效，取值为true 或 false|
|SetMute||||
|SetParameter|设置音效参数，可能会影响某些音效的播放效果|sound 音效名;param 参数名;value 参数值|无|
|SetVolume|设置音量|sound 音效名;volume 音量|无|

### Transform
管理物体的位置、大小、方向等。其中「大小」与AnimState有区别的地方在于，AnimState设置的大小是视觉上的大小，而Transform则是实际上的大小，会影响到物体的碰撞等相关物理效果。

|fn|用途|传入参数|返回值|
| :----: | :----: | :----: | :----: |
|GetFacing||||
|GetLocalPosition||||
|GetPredictionPosition||||
|GetRotation|获取物体的朝向|无|degree 朝向角度，取值范围[0,360]|
|GetScale|获取物体的缩放比例|无|x,y,z 物体在三个方向上的缩放比例|
|GetWorldPosition|获取物体的当前世界坐标|无|x,y,z 物体的三维坐标|
|SetEightFaced||||
|SetFourFaced|设置物体有四个面，会影响物体在相应朝向时展示的形态。四个面就是每个面占90度。其它类似的还有SetNoFaced(1面),SetTwoFaced(2面),SetSixFaced(6面),SetEightFaced(8面)|无|无|
|SetFromProxy|不确定，猜测是设置所有参数同步于proxy，一般常见于各种特效，同步于对应的附加物上|proxy_guid 常用的写法是传入proxy变量，然后取proxy.GUID|无|
|SetIsOnPlatform||||
|SetNoFaced||||
|SetPosition|设置物体的世界坐标，可以让物体瞬移到指定位置|x,y,z 物体的三维坐标|无|
|SetRotation|设置物体的朝向|degree 朝向角度，取值范围[0,360]|无|
|SetScale|设置物体的缩放比例|x,y,z 物体在三个方向上的缩放比例|无|
|SetSixFaced||||
|SetTwoFaced||||
|UpdateTransform||||

### VFXEffect
特殊音效管理

|fn|用途|传入参数|返回值|
| :----: | :----: | :----: | :----: |
|AddParticle||||
|AddParticleUV||||
|AddRotatingParticle||||
|AddRotatingParticleUV||||
|ClearAllParticles||||
|EnableBloomPass||||
|EnableDepthTest||||
|EnableDepthWrite||||
|FastForward||||
|GetNumLiveParticles||||
|InitEmitters||||
|SetAcceleration||||
|SetBlendMode||||
|SetColourEnvelope||||
|SetDragCoefficient||||
|SetFollowEmitter||||
|SetGroundPhysics||||
|SetIsTrailEmitter||||
|SetKillOnEntityDeath||||
|SetLayer||||
|SetMaxLifetime||||
|SetMaxNumParticles||||
|SetRadius||||
|SetRenderResources||||
|SetRotateOnVelocity||||
|SetRotationStatus||||
|SetScaleEnvelope||||
|SetSortOffset||||
|SetSortOrder||||
|SetSpawnVectors||||
|SetUVFrameSize||||
|SetWorldSpaceEmitter||||

### WaveComponent
海浪组件

|fn|用途|传入参数|返回值|
| :----: | :----: | :----: | :----: |
|Init||||
|SetWaveEffect||||
|SetWaveMotion||||
|SetWaveParams||||
|SetWaveSize||||
|SetWaveTexture||||

### TheInputProxy
全局变量，控制输入

|fn|用途|传入参数|返回值|
| :----: | :----: | :----: | :----: |
|AddVibration||||
|ApplyControlMapping||||
|CancelMapping||||
|EnableInputDevice||||
|EnableVibration||||
|FlushInput||||
|GetInputDeviceCount||||
|GetInputDeviceName||||
|GetInputDeviceType||||
|GetLastActiveControllerIndex||||
|GetLocalizedControl||||
|GetOSCursorPos||||
|HasMappingChanged||||
|IsAnyControllerActive||||
|IsAnyControllerConnected||||
|IsAnyInputDeviceConnected||||
|IsInputDeviceConnected||||
|IsInputDeviceEnabled||||
|LoadControls||||
|LoadCurrentControlMapping||||
|LoadDefaultControlMapping||||
|MapControl||||
|RemoveVibration||||
|SaveControls||||
|SetCursorVisible||||
|SetOSCursorPos||||
|StartMappingControls||||
|StopMappingControls||||
|StopVibration||||
|UnMapControl||||

### TheInventory
全局变量，与皮肤管理相关

|fn|用途|传入参数|返回值|
| :----: | :----: | :----: | :----: |
|CancelGetAllItems||||
|CheckClientOwnership||||
|CheckOwnership||||
|CheckOwnershipGetLatest||||
|GetAllUnlockedAchievements||||
|GetClientGiftCount||||
|GetCurrencyAmount||||
|GetFullInventory||||
|GetKleiPointsAmount||||
|GetLocalCookbook||||
|GetLocalPlantRegistry||||
|GetOwnedItemCount||||
|GetOwnedItemCountForCommerce||||
|GetUnopenedEntitlementItems||||
|GetUnopenedItems||||
|GetVirtualIAPCurrencyAmount||||
|GetWXP||||
|GetWXPLevel||||
|HasDownloadedInventory||||
|IsAchievementUnlocked||||
|IsDownloadingInventory||||
|LookupSkinname||||
|SetAchievementTempUnlocked||||
|SetCookbookValue||||
|SetItemOpened||||
|SetLocalVanityItems||||
|SetPlantRegistryValue||||
|StartGetAllItems||||

### TheNet
全局变量，与网络相关

|fn|用途|传入参数|返回值|
| :----: | :----: | :----: | :----: |
|AddToWhiteList||||
|AllowConnections||||
|Announce||||
|AnnounceDeath||||
|AnnounceResurrect||||
|AnnounceVoteResult||||
|AutoJoinLanServer||||
|Ban||||
|BanForTime||||
|BeginServerModSetup||||
|BeginSession||||
|CallClientRPC||||
|CallRPC||||
|CallShardRPC||||
|CancelCloudServerRequest||||
|CleanupSessionCache||||
|DeleteCluster||||
|DeleteSession||||
|DeleteUserSession||||
|DeserializeAllLocalUserSessions||||
|DeserializeUserSession||||
|DeserializeUserSessionInClusterSlot||||
|DiceRoll||||
|Disconnect||||
|DoneLoadingMap||||
|DownloadServerDetails||||
|DownloadServerMods||||
|EncodeUserPath||||
|GenerateClusterToken||||
|GetAllowIncomingConnections||||
|GetAllowNewPlayersToConnect||||
|GetAutosaverEnabled||||
|GetAveragePing||||
|GetBlacklist||||
|GetChildProcessError||||
|GetChildProcessStatus||||
|GetClientMetricsForUser||||
|GetClientTable||||
|GetClientTableForUser||||
|GetCloudServerId||||
|GetCloudServerRequestState||||
|GetCountryCode||||
|GetCurrentSnapshot||||
|GetDefaultClanAdmins||||
|GetDefaultClanID||||
|GetDefaultClanOnly||||
|GetDefaultEncodeUserPath||||
|GetDefaultFriendsOnlyServer||||
|GetDefaultGameMode||||
|GetDefaultLANOnlyServer||||
|GetDefaultMaxPlayers||||
|GetDefaultPvpSetting||||
|GetDefaultServerDescription||||
|GetDefaultServerIntention||||
|GetDefaultServerLanguage||||
|GetDefaultServerName||||
|GetDefaultServerPassword||||
|GetDefaultVoteEnabled||||
|GetDeferredServerShutdownRequested||||
|GetFriendsList||||
|GetIsClient||||
|GetIsHosting||||
|GetIsMasterSimulation||||
|GetIsServer||||
|GetIsServerAdmin||||
|GetIsServerOwner||||
|GetItemsBranch||||
|GetLanguageCode||||
|GetLocalUserName||||
|GetNetworkStatistics||||
|GetPVPEnabled||||
|GetPartyChatHistory||||
|GetPartyTable||||
|GetPing||||
|GetPlayerCount||||
|GetPlayerSaveLocationInClusterSlot||||
|GetServerClanID||||
|GetServerClanOnly||||
|GetServerDescription||||
|GetServerEvent||||
|GetServerFriendsOnly||||
|GetServerGameMode||||
|GetServerHasPassword||||
|GetServerHasPresentAdmin||||
|GetServerIntention||||
|GetServerIsClientHosted||||
|GetServerIsDedicated||||
|GetServerLANOnly||||
|GetServerListing||||
|GetServerListingFromActualIndex||||
|GetServerListingReadDirty||||
|GetServerListings||||
|GetServerMaxPlayers||||
|GetServerModNames||||
|GetServerModsDescription||||
|GetServerModsEnabled||||
|GetServerName||||
|GetServerPVP||||
|GetSessionIdentifier||||
|GetUserID||||
|GetUserSessionFile||||
|GetUserSessionFileInClusterSlot||||
|GetWorldSessionFile||||
|GetWorldSessionFileInClusterSlot||||
|HasPendingConnection||||
|IncrementSnapshot||||
|InviteToParty||||
|IsClanIDValid||||
|IsConsecutiveMatchForPlayer||||
|IsDedicated||||
|IsDedicatedOfflineCluster||||
|IsNetIDPlatformValid||||
|IsNetOverlayEnabled||||
|IsOnlineMode||||
|IsSearchingServers||||
|IsVoiceActive||||
|IsWhiteListed||||
|JoinParty||||
|JoinServerResponse||||
|Kick||||
|LeaveParty||||
|ListSnapshots||||
|ListSnapshotsInClusterSlot||||
|LoadPermissionLists||||
|NotifyAuthenticationFailure||||
|NotifyLoadingState||||
|OnPlayerHistoryUpdated||||
|PartyChat||||
|PrintNetwork||||
|RemoveFromWhiteList||||
|ReportListing||||
|Say||||
|SearchLANServers||||
|SearchServers||||
|SendLobbyCharacterRequestToServer||||
|SendModRPCToClient||||
|SendModRPCToServer||||
|SendModRPCToShard||||
|SendRPCToClient||||
|SendRPCToServer||||
|SendRPCToShard||||
|SendRemoteExecute||||
|SendResumeRequestToServer||||
|SendSlashCmdToServer||||
|SendSpawnRequestToServer||||
|SendWorldResetRequestToServer||||
|SendWorldRollbackRequestToServer||||
|SendWorldSaveRequestToMaster||||
|SerializeUserSession||||
|SerializeWorldSession||||
|ServerModCollectionSetup||||
|ServerModSetup||||
|ServerModsDownloadCompleted||||
|SetAllowIncomingConnections||||
|SetAllowNewPlayersToConnect||||
|SetBlacklist||||
|SetCheckVersionOnQuery||||
|SetClientCacheSessionIdentifier||||
|SetCloudServerInitiatorUserId||||
|SetCurrentSnapshot||||
|SetDefaultClanInfo||||
|SetDefaultFriendsOnlyServer||||
|SetDefaultGameMode||||
|SetDefaultLANOnlyServer||||
|SetDefaultMaxPlayers||||
|SetDefaultPvpSetting||||
|SetDefaultServerDescription||||
|SetDefaultServerIntention||||
|SetDefaultServerLanguage||||
|SetDefaultServerName||||
|SetDefaultServerPassword||||
|SetDeferredServerShutdownRequested||||
|SetGameData||||
|SetIsClientInWorld||||
|SetIsMatchStarting||||
|SetIsWorldResetting||||
|SetIsWorldSaving||||
|SetLobbyCharacter||||
|SetPartyServer||||
|SetPlayerMuted||||
|SetSeason||||
|SetServerPassword||||
|SetServerTags||||
|SetWorldGenData||||
|StartClient||||
|StartCloudServerRequestProcess||||
|StartServer||||
|StartVote||||
|StopBroadcastingServer||||
|StopSearchingServers||||
|StopVote||||
|SystemMessage||||
|Talker||||
|TruncateSnapshots||||
|TruncateSnapshotsInClusterSlot||||
|TryDefaultEncodeUserPath||||
|UpdatePlayingWithFriends||||
|ViewNetFriends||||
|ViewNetProfile||||
|Vote||||

### TheShard
全局变量，当前分片，比如地穴就是一个独立分片

|fn|用途|传入参数|返回值|
| :----: | :----: | :----: | :----: |
|GetDefaultShardEnabled||||
|GetSecondaryShardPlayerCounts||||
|GetShardId||||
|IsMaster||||
|IsMigrating||||
|IsPlayer||||
|IsSecondary||||
|SetSecondaryLoading||||
|StartMigration||||

### TheSim
全局变量，游戏系统本身

|fn|用途|传入参数|返回值|
| :----: | :----: | :----: | :----: |
|AbortFileExistsAsync||||
|AddBatchVerifyFileExists||||
|AdjustFontAdvance||||
|AtlasContains||||
|CanReadConfigurationDirectory||||
|CanWriteConfigurationDirectory||||
|CheckPersistentStringExists||||
|CleanAllMods||||
|ClearAllDSP||||
|ClearDSP||||
|ClearFileSystemAliases||||
|ClearInput||||
|CopyLegacySessionToSlot||||
|CreateEntity||||
|DebugPause||||
|DebugPushJsonMessage||||
|DebugStringScreen||||
|DecodeAndUnzipString||||
|DecodeKleiData||||
|DownloadMOTDImages||||
|DumpMemInfo||||
|DumpMemoryStats||||
|EnsureShardIndexPathExists||||
|ErasePersistentString||||
|FindEntities||||
|FindEntities_Registered||||
|FindFirstEntityWithTag||||
|ForceAbort||||
|GenerateNewWorld||||
|GetAnalogControl||||
|GetBuildDate||||
|GetClientModsDownloading||||
|GetClipboardData||||
|GetDataCollectionSetting||||
|GetDebugPhysicsRenderEnabled||||
|GetDebugRenderEnabled||||
|GetDigitalControl||||
|GetEntitiesAtScreenPoint||||
|GetEntityAtScreenPoint||||
|GetFPS||||
|GetFileModificationTime||||
|GetGameID||||
|GetGroundViewDirection||||
|GetLightAtPoint||||
|GetLocalSetting||||
|GetMOTDQueryURL||||
|GetModDirectoryNames||||
|GetMouseButtonState||||
|GetNumLaunches||||
|GetNumberOfEntities||||
|GetPersistentString||||
|GetPersistentStringInClusterSlot||||
|GetPosition||||
|GetRealTime||||
|GetSaveFiles||||
|GetScreenPos||||
|GetScreenSize||||
|GetServerModsDownloading||||
|GetSetting||||
|GetSoundVolume||||
|GetStashedPlayInstance||||
|GetSteamAppID||||
|GetSteamBetaBranchName||||
|GetSteamIDNumber||||
|GetStep||||
|GetTick||||
|GetTickTime||||
|GetTimeScale||||
|GetUserHasLicenseForApp||||
|GetUsersName||||
|GetWindowSize||||
|GetWorkshopVersion||||
|HasEnoughFreeDiskSpace||||
|HasPlayerSkeletons||||
|HasWindowFocus||||
|HideAnimOnEntitiesWithTag||||
|Hook||||
|IsBorrowed||||
|IsDLCEnabled||||
|IsDLCInstalled||||
|IsDataCollectionDisabled||||
|IsDebugPaused||||
|IsKeyDown||||
|IsLoggedOn||||
|IsNetbookMode||||
|IsPlaying||||
|LoadFont||||
|LoadKlumpFile||||
|LoadKlumpString||||
|LoadPrefabs||||
|LoadUserFile||||
|LockModDir||||
|LogBulkMetric||||
|LuaPrint||||
|MemTrackerPop||||
|MemTrackerPush||||
|OnAssetPathResolve||||
|OpenDocumentsFolder||||
|OpenSaveFolder||||
|PauseFileExistsAsync||||
|PreloadFile||||
|PrintLoadedTextureInfo||||
|PrintTextureInfo||||
|Profile||||
|ProfilerPop||||
|ProfilerPush||||
|ProjectScreenPos||||
|QueryServer||||
|QueryTopMods||||
|QueryWorkshopModName||||
|QueueDownloadTempMod||||
|Quit||||
|RegisterFindTags||||
|RegisterPrefab||||
|RemapSoundEvent||||
|RenderOneFrame||||
|ReportAction||||
|RequestPlayerID||||
|Reset||||
|ReskinEntity||||
|SendGameStat||||
|SendHardwareStats||||
|SendJSMessage||||
|SendProfileStats||||
|SendUITrigger||||
|SetActiveAreaCenterpoint||||
|SetAmbientColour||||
|SetCameraDir||||
|SetCameraFOV||||
|SetCameraPos||||
|SetCameraUp||||
|SetDLCEnabled||||
|SetDataCollectionSetting||||
|SetDebugCameraRotation||||
|SetDebugCameraTarget||||
|SetDebugPhysicsRenderEnabled||||
|SetDebugRenderEnabled||||
|SetErosionTexture||||
|SetHighPassFilter||||
|SetInstanceParameters||||
|SetListener||||
|SetLowPassFilter||||
|SetMOTDTarget||||
|SetMemInfoTrackingInterval||||
|SetMemoryTracking||||
|SetNetbookMode||||
|SetPersistentString||||
|SetPersistentStringInClusterSlot||||
|SetRenderPassDefaultEffect||||
|SetReverbPreset||||
|SetSetting||||
|SetSoundVolume||||
|SetTimeScale||||
|SetUIRoot||||
|SetVisualAmbientColour||||
|SetupFontFallbacks||||
|ShouldInitDebugger||||
|ShouldPlayIntroMovie||||
|ShouldWarnModsLoaded||||
|ShowAnimOnEntitiesWithTag||||
|SpawnPrefab||||
|StartDownloadTempMods||||
|StartFileExistsAsync||||
|StartWorkshopQuery||||
|StashPlayInstance||||
|Step||||
|StopAllSounds||||
|SubscribeToMod||||
|ToggleDataProfiler||||
|ToggleDebugCamera||||
|ToggleDebugPause||||
|ToggleDebugTexture||||
|ToggleFrameProfiler||||
|TogglePerfGraph||||
|TryLockModDir||||
|TurnOffReverb||||
|UnloadAllPrefabs||||
|UnloadFont||||
|UnloadPrefabs||||
|UnlockModDir||||
|UnregisterAllPrefabs||||
|UnregisterPrefabs||||
|UpdateDebugTexture||||
|UpdateDeviceCaps||||
|UpdateWorkshopMod||||
|UserChooseDirectory||||
|ValidateHeap||||
|VerifyFileExistsAsync||||
|VerifyModVersions||||
|WorldPointInPoly||||
|ZipAndEncodeString||||
