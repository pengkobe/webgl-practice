### 着色器程序的创建
流程实在复杂，做点笔记
```
// 添加DOM节点
<canvas id="canvas" />

//获取WebGL对象
var webgl=canvas.getContext("experimental-webgl");
//创建程序（仅仅是创建而已）
var program=webgl.createProgram();
var vs,fs,vs_s,fs_s;

//创建顶点着色器和片段着色器
vs=webgl.createShader(webgl.VERTEX_SHADER);
fs=webgl.createShader(webgl.FRAGMENT_SHADER);

/* 着色器程序的源码
   vec4:其实是矩阵的缩写(x,y,z,w),w代表缩放，
           无论是坐标的线性变换还是颜色的光照变换都可以用矩阵和向量相乘来完成（线性代数）  
   attribute\uniform\varying：顶点数据源(值或者数组)\xx\定义变量
*/
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
```
//把程序放入显存中
webgl.useProgram(program);
//定义一个顶点数组，这这里有三个坐标
var dat=[0,1,0,1,  -1,-1,0,1,  1,-1,0,1];

//将JavaScript的哈希数组转换为连续的内存数组
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

```
