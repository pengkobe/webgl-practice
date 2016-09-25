# 次碳酸钴关于webgl的专题文章学习笔记
次碳酸钴，饿了么90后架构师，坚持在博客分享多年，对众多web技术都有独到的见解，webgl专题是他在13年写的系列文章。
共22篇。

### 着色器程序的创建
着重描述写webgl着色器程序繁杂的流程，并提供一个HelloWorld程序

```javascript
var vs,fs,vs_s,fs_s;

//创建顶点着色器和片段着色器
vs=webgl.createShader(webgl.VERTEX_SHADER);
fs=webgl.createShader(webgl.FRAGMENT_SHADER);

//着色器程序的源码
vs_s="attribute vec4 p;void main(){gl_Position=p;}";
fs_s="void main(){gl_FragColor=vec4(1.0,0.0,0.0,1.0);}";

//把源码添加进着色器
webgl.shaderSource(vs,vs_s);
webgl.shaderSource(fs,fs_s);

//编译着色器
webgl.compileShader(vs);
webgl.compileShader(fs);

//把着色器添加到程序中
webgl.attachShader(program,vs);
webgl.attachShader(program,fs);

//把这两个着色器程序链接成一个完整的程序
webgl.linkProgram(program);
```

### 着色器程序的使用(绘图)
讲解向量代表的含义，以及顶点着色器中是可以使用“attribute”关键字定义变量p,p是怎样传递给显卡的呢，步骤很繁杂，见代码

```javascript
//把这个程序放入显存中
webgl.useProgram(program);

//定义一个顶点数组，这这里有三个坐标
var dat=[0,1,0,1,  -1,-1,0,1,  1,-1,0,1];

//将JavaScript的哈希数组转换为连续的内存数组，Float32Array为强类型数组
dat=new Float32Array(dat);

//在显存中建立一个数据缓冲区
var buf=webgl.createBuffer();

//设置这个数据缓冲区为当前操作对象
webgl.bindBuffer(webgl.ARRAY_BUFFER,buf);

//把内存中的顶点数组复制到当前操作的数据缓冲区中
webgl.bufferData(webgl.ARRAY_BUFFER,dat,webgl.STATIC_DRAW);

//获取“顶点数据源”接口p的序号
var p=webgl.getAttribLocation(program,"p");

//开启p的数组模式
webgl.enableVertexAttribArray(p);

//把当前工作的数据缓冲区指定给p这个接口
webgl.vertexAttribPointer(p,4,webgl.FLOAT,false,0,0);

//绘制
webgl.drawArrays(webgl.TRIANGLES,0,3);
```

### 关于图元与索引的绘制
详细讲解怎样使用图元绘图，如正方体、球体等，所有得图元列表如下:
* POINTS：只绘制顶点
* LINE_STRIP：把每个顶点按顺序用线连起来
* LINE_LOOP：同上，并且头尾相连
* LINES：每两个顶点相连
* TRIANGLE_STRIP：从第三个顶点开始，每个顶点与前两个顶点相连形成三角形
* TRIANGLE_FAN：从第三个顶点开始，每个顶点与第一个和上一个顶点连成三角形
* TRIANGLES：每三个顶点连成三角形

```javascript
//开始的一大坨代码

//定义一个顶点数组，为了构造矩形需要四个顶点
var dat=[-0.5,-0.5,0,1 ,-0.5,0.5,0,1 ,0.5,-0.5,0,1 ,0.5,0.5,0,1];

//中间的一大坨代码

//把索引放入显存
var index=webgl.createBuffer();
webgl.bindBuffer(webgl.ELEMENT_ARRAY_BUFFER,index);
webgl.bufferData(
  webgl.ELEMENT_ARRAY_BUFFER,
  new Uint16Array([0,1,2,1,2,3]),
  webgl.STATIC_DRAW
);
//绘制索引（注意索引数据必须在当前工作缓冲区中）
webgl.drawElements(webgl.TRIANGLES,6,webgl.UNSIGNED_SHORT,0);
```

### 矩阵与着色器的内部工作
怎样画一个各面颜色不同的正方体并进行旋转，分三部分进行讲解
1. 指定数据源
2. 设置uniform代码
3. 绘制

### 最简单的贴图
贴图可以使几何体真正具有实用价值，重点是需要指定贴图坐标坐标数据源

```javascript
for(dat=[],i=0;i<tmp.length;i+=3)for(j=0;j<3;j++)
  if(j!=(i/24|0))dat.push((tmp[i+j]+1)/2);
```

以及如何设置图片数据源

```javascript
//设置图片数据
var img;
img=new Image;
img.src="/images/signet-128.png";
img.onload=function(){
  //创建贴图（材质）对象句柄
  var texture=webgl.createTexture();
  //指定当前使用的贴图序号
  webgl.activeTexture(webgl.TEXTURE0);
  //设置为当前工作的贴图对象
  webgl.bindTexture(webgl.TEXTURE_2D,texture);
  //设置图片数据
  webgl.texImage2D(
    webgl.TEXTURE_2D,0,webgl.RGBA,webgl.RGBA,webgl.UNSIGNED_BYTE,img
  );
  //设置贴图参数
  webgl.texParameteri(
    webgl.TEXTURE_2D,webgl.TEXTURE_MIN_FILTER,webgl.NEAREST
  );
  //把预置缓冲区的序号传入uniform
  webgl.uniform1i(tex,0);
  //开始绘制（这个是自定义函数，在后面定义的）
  draw();
};
```

