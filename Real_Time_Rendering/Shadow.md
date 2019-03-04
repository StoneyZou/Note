## Shadow
- 阴影场景的组成：
    - light source(光源)
    - occluder(阻挡物,shadow cast)
    - receiver(接收面,shadow receive)
    - shadow(阴影)
        - umbre(硬阴影部分)
        - penumber(软阴影部分)
![Figure1](http://img2.ph.126.net/fOd7xcND62XVttMDNKNzcQ==/6631901195515762091.png)
- 生成阴影常见的几个问题：
    - Self Shadow(自阴影，物体被自己产生的阴影覆盖)
    - Anti-Shadow(反阴影，在光源之上的物体产生阴影)
    - Light Bleed(漏光，由于精度问题导致应该被阴影覆盖的地方没有被覆盖上。Self Shdow实际上也是一种Light Bleed)。

### Planer Shadow(平面阴影)
- 原理：如果阴影只在一个相同的水平面上被接收，这是阴影的形状就是Mesh在光照方向上的投影。
- 根据三角形等比原理，可以得出
$$
    T_{dst}=T_{src}+V_{xy}/V_z*H
$$
- 进一步可以推出
$$
    p=l-\frac{{n}\cdot{l}+{d}}{n\cdot{(l-v)}}(l-v)
$$
$$
    p=\frac{l(n\cdot{l})-l(n\cdot{v})-l(n\cdot{l})-ld+v(n\cdot{l}+d)}{n\cdot{l}-n\cdot{v}}
$$
$$
    p=\frac{v(n\cdot{l}+d)-l(n\cdot{v})-ld}{n\cdot{l}-n\cdot{v}}
$$
-转换为矩阵M，Mv=p
$$
    M=
        \begin{bmatrix}
            (n\cdot{l}+d)-l_xn_x&-l_xn_y&-l_xn_z&-l_xd\\
            -l_yn_x&(n\cdot{l}+d)-l_yn_y&-l_yn_z&-l_yd\\
            -l_zn_x&-l_zn_y&(n\cdot{l}+d)-l_zn_z&-l_zd\\
            -n_x&-n_y&-n_z&n\cdot{l}
        \end{bmatrix}
$$
- 优点：
    计算速度快，不需要有计算以外的开销。使用时只需要对需要投影阴影的Mesh的顶点经过矩阵转换后重新渲染一遍即可。适用于地形大部分为平面的场景。
- 缺点：阴影只能投影在一个固定平面上，不能处理曲面阴影

### Shadow Volume(阴影体)
- 原理：Mesh由多个三角形面片组成，由点光源对三角形面片上的各个顶点发出一条射线，可以组成一个底在无穷远的三角体，去掉光源与三角形面片之间的体积，余下部分称为Shadow Volume。如果一个点处于Shadow Volume内，那么这个点是被阴影覆盖的。
- 实际运用：渲染occluder时通过geometry shader生成阴影体网格并进行渲染。如果屏幕上某个点经过Shadow Volume正面时+1，经过Shadow Volume反面时-1(可以通过Stencil Buffer以及Z—Pass来达到该效果)，最终值大于0的点即为处于Shadow Volume内。
- 问题：Stencil Buffer的位数不足以存下Shadow Volume计数。
    - 解决方法：使用一张额外的Texture存放Shadow Volume计数。把Shadow Volume的渲染拆开为两个pass，先渲染Front Shadow Volume，后Back Shadow Volume(避免Texture不支持signed number的原因，其实使用Texure取值范围中位数作为Clear值就好)
- Z-Pass Shadow Volume：
    1. Stencil Clear
    2. 渲染一遍场景到Z-Buffer
    3. 渲染Front Shadow Volume，比较Volume的Z值与Depth-Buffer的Z值，如果Volume的Z值更小或者相等则+1(Z-Pass)
    4. 渲染Back Shadow Volume，比较Volume的Z值与Depth-Buffer的Z值，如果Volume的Z值更大则-1
    5. 渲染整个屏幕，把Stencil Buffer上大于1的点渲染为阴影
- Z-Pass Shadow Volume的缺点:
    - 不能处理相机屏幕的一部分处于Shadow Volume中的情况
- Z-Fail Shadow Volume:
    1. Stencil Clear
    2. 渲染一遍场景到Z-Buffer
    3. 渲染Back Shadow Volume，比较Volume的Z值与Depth-Buffer的Z值，如果Volume的Z值更大则+1(Z-Fail)
    4. 渲染Front Shadow Volume，比较Volume的Z值与Depth-Buffer的Z值，如果Volume的Z值更小或者相等则-1
    5. 渲染整个屏幕，把Stencil Buffer上大于0的点渲染为阴影
- Z-Fail Shadow Volume的缺点:
    - 避免了Z-Pass的问题，但是不能Back Shadow Volume一部分被Far Panel裁剪掉导致引用计数出错的问题。