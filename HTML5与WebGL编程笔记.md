## HTML5与WebGL编程笔记
网购该书，甚是兴奋，开看！不过和之前自己找的网络资料有很多重复之处，再看一遍就当巩固复习了吧。

### 绪论
3d技术的发展过程是曲折的，直至今日才逐渐成熟，html5对3d的应用加快了该技术的普及。
#### 3d图形基本知识
1. 什么是3d
额，想想就明白了
2. 3d坐标系
需注意3d坐标系与canvas坐标系的区别，一个向上一个向下。
3. 网格、多边形与顶点
这几个要素构成3d模型，但不决定色彩与明暗
4. 材质、纹理与光源
5.变换与矩阵
可参考:http://www.songho.ca/opengl/gl_transform.html
6. 相机、透视、视口与投影
透视(即远处的物体看起来比较小).投影矩阵(3d坐标到视口的2d绘制空间坐标的转换)
7. 着色器
材质、光源、变换以及相机由其处理，css3滤镜与webgl绘制都依赖于着色器，2d cavas api是不支持的。

### WebGL: 实时3d渲染
#### 步骤
1. 创建一个canvas元素
2. 获取绘图上下文
3. 初始化视口
4. 创建一个或多个包含待渲染数据的缓冲(一般是顶点)
5. 创建一个或多个顶点缓冲到屏幕空间转换规则的矩阵
6. 创建一个或多个实现绘制算法的着色器
7. 使用各项参数初始化着色气器
8. 绘制

#### 索引缓冲
能够避免数据重复，令数据存储更加紧凑，但是索引的构造原理是什么？

####  开启深度测试
其按照深度排序来绘制立方体，不开启的话无法区分"前方"和"后方"，造成图形显示不全
```javascript
gl.enable(gl.DEPTH_TEST);
```

#### 纹理
纹理映射使用到图片时必须基于web服务器，其对跨域做了一定要求，觉得很有趣的是其可以使用视频作为构建源
```javascript
//绑定纹理
gl.bindTexture(gl.TEXTURE_2D, texture);
// 翻转纹理坐标
gl.pixelStorei(gl.UNPACK_FLIP_Y_WEBGL, true);
// 复制图像到纹理对象
gl.texImage2D(gl.TEXTURE_2D, 0, gl.RGBA, gl.RGBA, gl.UNSIGNED_BYTE, texture.image);
// 纹理计算方式
gl.texParameteri(gl.TEXTURE_2D, gl.TEXTURE_MAG_FILTER, gl.NEAREST);
gl.texParameteri(gl.TEXTURE_2D, gl.TEXTURE_MIN_FILTER, gl.NEAREST);
// 防止意外更改存储区域
gl.bindTexture(gl.TEXTURE_2D, null);
```

### threejs简介
从这个游戏的效果来说还是很不错的：http://hexgl.bkcore.com/play/
此外对比下代码量，瞬间信心上来了。

### threejs中的图形与渲染







