### Bump Mapping(凹凸贴图)
- 背景：在面数比较低的模型上，位于同一个面上的像素使用同一法线导致看起来没有立体感(凹凸感)。
- 思想：使用一张贴图保存模型表面的高度信息(高度图)，根据高度图对法线信息进行计算，使用计算后的法线进行光照计算。
- 优点(相对于Normal Mapping)
    - 高度贴图只保存高度信息使用的内存比Normal Mapping少，在ram资源比较稀缺的时候适用。现今已不适用该方法。
- 缺点(相对于Normal Mapping)
    - 需要逐像素计算法线

### Normal Mapping(法线贴图)
- 思想：Bump Mapping的进一步预计算结果，直接把法线保存在贴图上。
- 优点
    - 不需要逐像素计算法线，只需要读取贴图上的数据使用。
- 缺点
    - 需要保存的信息量较Bump Mapping大
    - 在视角与平面法线夹角比较大时，平面的立体感不强(seem  unreal)

### Parallax Mapping(视差贴图)
- 背景：在视角与平面法线夹角比较大时，仅仅使用Normal Maping是不够的。因为由于平面上的像素之间是有高度关系，所以像素之间可能会发生遮挡问题，这就导致了视线与物体表面相交点与现实中真正应该看到的点不是同一个点。
- 思想：根据视线与平面焦点的像素高度，对采样点进行偏移计算，从而得出一个近似正确的采样点。
- 公式：(需转换到切平面上计算，切空间x轴,y轴在切平面上，z轴垂直于切平面，H为相对于切平面的高度，V为视线在切线空间的方向，$V_{z}$为视线与切平面的夹角，$V_{z}$越大夹角越小偏移越小，$V_{z}$越小夹角越大偏移越大)
    - $T_{dst}=T_{src}+V_{xy}/V_z*H$
    - $T_{dst}=T_{src}+V_{xy}*H$（避免视线与切平面夹角太小导致$V_z$太小值太大）

![图1](https://sfault-image.b0.upaiyun.com/167/480/1674800956-56304f7d8f965)

![图1](https://segmentfault.com/img/bVqBOc)

### Relief Mapping(浮雕贴图)
- 背景：视差贴图结果只是近似值在物体表面比较凹凸不平的时候得出的效果并不是很好。
- 思想：在视差贴图的思想上改进。逐层递增计算一个近似点的上下区间，再在这个区间内二分查找更为准确的采样点。


![图1](https://segmentfault.com/img/bVqBQs)

![图1](https://segmentfault.com/img/bVqBVE)

### 引用
> 1. https://segmentfault.com/a/1190000003920502
> 2. http://www.opengpu.org/forum.php?mod=viewthread&tid=362
> 3. Real Time Renderer 6.7 p183