## Shadow Map Aliasing
- **原因：** 引起Shadow Map Aliasing的原因很多，除了硬件浮点精度问题以外，也有软件上Shadow Map到场景的映射系数太大的原因。其中又分为Perspective Aliasing和Projection Aliasing，这两种Aliasing的原因是因为Light视锥和Camera视锥不匹配有关。在应用上要避免出现Shadow Map Aliasing需要减少Shadow Map到场景的映射系数。
- **映射系数：** ![Figure1](http://img1.ph.126.net/EMLJ-80hgyUVJIZI41p1Ng==/6632373985513298950.png)
- **映射系数公式(点光源)**
$$
    k=\frac{d_s*r_s/cos\alpha}{d_i*r_i/cos\beta}=\frac{d_s}{d_i}\frac{r_s}{r_i}\frac{cos\beta}{cos\alpha}
$$
- **Perspective Aliasing：** 
    - 由于Camera距离某个点较近，光源距离某个点较远，导致$\frac{r_s}{r_i}$过大导致的Aliasing，称为Perspective Aliasing
- **Projection Aliasing：** 
    - 由于Camera平面与某个点的法线夹较大，光源平面与某个点的法线夹较大，导致$\frac{cos\beta}{cos\alpha}$过大导致的Aliasing，称为Projection Aliasing
