... 高动态UI优化
.. 背景：大部分游戏在战斗场景中hud属于高动态UI可能每几帧就会发生一次变化，而战斗场景属于性能敏感的场景，因此降低HUD本身的性能消耗对良好的游戏体验有重要影响
- 目标：
	- 1、更新性能消耗低
		- Tranform更新
		- Alpha更新
		- Size更新
		- Depth更新
	- 2、满足UI所需的基本功能
	- 3、提供碎片化UI动态Pack需求
- Unity UI SDK目前使用现状 NGUI vs UGUI：
	- 优点：通用、成熟、灵活、满足大部分静态和低动态UI需求
	- 缺点：没有太多针对高动态UI的优化
- 自定义UI底层 3DUI SDK：
	- 