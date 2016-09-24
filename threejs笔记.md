## threejs学习笔记
本人主要按照\<\<ThreeJS入门指南\>\>(免费)来进行学习。
网址: https://read.douban.com/reader/ebook/7412854/


### threejs
代码量显著减少是其最明显的一个特征，如绘制三角形代码只有区区30行，而原生webgl代码达到5倍多
```javascript
var renderer = new THREE.WebGLRenderer({
    canvas: document.getElementById('mainCanvas')
});
renderer.setClearColor(0x000000); // black

var scene = new THREE.Scene();

var camera = new THREE.PerspectiveCamera(45, 4 / 3, 1, 1000);
camera.position.set(0, 0, 5);
camera.lookAt(new THREE.Vector3(0, 0, 0));
scene.add(camera);

var material = new THREE.MeshBasicMaterial({
        color: 0xffffff // white
});
// plane
var planeGeo = new THREE.PlaneGeometry(1.5, 1.5);
var plane = new THREE.Mesh(planeGeo, material);
plane.position.x = 1;
scene.add(plane);

// triangle
var triGeo = new THREE.Geometry();
triGeo.vertices = [new THREE.Vector3(0, -0.8, 0),
        new THREE.Vector3(-2, -0.8, 0), new THREE.Vector3(-1, 0.8, 0)];
triGeo.faces.push(new THREE.Face3(0, 2, 1));
var triangle = new THREE.Mesh(triGeo, material);
scene.add(triangle);

renderer.render(scene, camera);
```

### threejs功能预览
一眼望去实在有点多，可以一个个进行了解，实践出真知。
```
Cameras（照相机，控制投影方式）
    Camera
       OrthographicCamera (正交投影，参数:left, right, top, bottom, near, far)
       PerspectiveCamera (透视投影，参数:fov[视景体竖直方向上的张角], aspect[水平方向和竖直方向长度的比值], near, far)

Core（核心对象）
    BufferGeometry
    Clock（用来记录时间）
    EventDispatcher
    Face3
    Face4
    Geometry//创建自定义形状[new THREE.Face4(0, 1, 2, 3);//面片  new THREE.Vector3(-1, 2, -1)//顶点]
    Object3D
    Projector
    Raycaster（计算鼠标拾取物体时很有用的对象）

Lights（光照）
    Light
    AmbientLight
    AreaLight
    DirectionalLight
    HemisphereLight
    PointLight
    SpotLight

Loaders（加载器，用来加载特定文件）
    Loader
    BinaryLoader
    GeometryLoader
    ImageLoader
    JSONLoader
    LoadingMonitor
    SceneLoader
    TextureLoader(加载纹理)

Materials（材质，控制物体的颜色、纹理等）
    Material
    LineBasicMaterial
    LineDashedMaterial
    MeshBasicMaterial(渲染后物体的颜色始终为该材质的颜色，而不会由于光照产生明暗、阴影效果。如果没有指定材质的颜色，则颜色是随机的)
    MeshDepthMaterial
    MeshFaceMaterial
    MeshLambertMaterial(只考虑漫反射而不考虑镜面反射的效果，对金属、镜子等需要镜面反射效果的物体不适应，对于其他大部分物体的漫反射效果适用)
    MeshNormalMaterial(颜色随角度改变，知道物体的法向量，使用法向材质就很有效)
    MeshPhongMaterial(考虑了镜面反射的效果，因此对于金属、镜面的表现尤为适合)
    ParticleBasicMaterial
    ParticleCanvasMaterial
    ParticleDOMMaterial
    ShaderMaterial
    SpriteMaterial

Math（和数学相关的对象）
    Box2
    Box3
    Color
    Frustum
    Math
    Matrix3
    Matrix4
    Plane
    Quaternion
    Ray
    Sphere
    Spline
    Triangle
    Vector2
    Vector3
    Vector4

Objects（物体）
    Bone
    Line
    LOD
    Mesh（网格，最常用的物体,参数:geometry, material）
    MorphAnimMesh
    Particle
    ParticleSystem
    Ribbon
    SkinnedMesh
    Sprite

Renderers（渲染器，可以渲染到不同对象上）
CanvasRenderer
    WebGLRenderer（使用WebGL渲染，这是本书中最常用的方式）
    WebGLRenderTarget
    WebGLRenderTargetCube
    WebGLShaders（着色器，在最后一章作介绍）

Renderers / Renderables
    RenderableFace3
    RenderableFace4
    RenderableLine
    RenderableObject
    RenderableParticle
    RenderableVertex

Scenes（场景）
    Fog
    FogExp2
    Scene

Textures（纹理）
    CompressedTexture
    DataTexture
    Texture

Extras
    FontUtils
    GeometryUtils
    ImageUtils
    SceneUtils

Extras / Animation
    Animation
    AnimationHandler
    AnimationMorphTarget
    KeyFrameAnimation

Extras / Cameras
CombinedCamera
    CubeCamera

Extras / Core
    Curve
    CurvePath
    Gyroscope
    Path
    Shape

Extras / Geometries(几何形状)
    CircleGeometry(圆形,参数:radius, segments, thetaStart, thetaLength)
    ConvexGeometry
    CubeGeometry(立方体,参数:width, height, depth, widthSegments, heightSegments, depthSegments)
    CylinderGeometry(圆柱体,参数:radiusTop, radiusBottom, height, radiusSegments, heightSegments, openEnded)
    ExtrudeGeometry
    IcosahedronGeometry(正12面体,参数:radius, detail)
    LatheGeometry
    OctahedronGeometry(正8面体,参数:radius, detail)
    ParametricGeometry
    PlaneGeometry(面,参数:width, height, widthSegments, heightSegments)
    PolyhedronGeometry
    ShapeGeometry(球面,参数:radius, segmentsWidth, segmentsHeight, phiStart, phiLength, thetaStart, thetaLength)
    SphereGeometry
    TetrahedronGeometry(正4面体,参数:radius, detail)
    TextGeometry(文字形状,参数:text, parameters)
    TorusGeometry(圆环面,参数:radius, tube, radialSegments, tubularSegments, arc)
    TorusKnotGeometry(圆环节,参数:radius, tube, radialSegments, tubularSegments, p, q, heightScale)
    TubeGeometry

Extras / Helpers
    ArrowHelper
    AxisHelper
    CameraHelper
    DirectionalLightHelper
    HemisphereLightHelper
    PointLightHelper
    SpotLightHelper

Extras / Objects
    ImmediateRenderObject
    LensFlare
    MorphBlendMesh

Extras / Renderers / Plugins
    DepthPassPlugin
    LensFlarePlugin
    ShadowMapPlugin
    SpritePlugin

Extras / Shaders
    ShaderFlares
    ShaderSprite
```

### 六张图像应用于长方形
```javascript
var materials = [];
for (var i = 0; i < 6; ++i) {
    materials.push(new THREE.MeshBasicMaterial({
        map: THREE.ImageUtils.loadTexture('../img/' + i + '.png',
                {}, function() {
                    renderer.render(scene, camera);
                }),
        overdraw: true
    }));
}
var cube = new THREE.Mesh(new THREE.CubeGeometry(5, 5, 5),
        new THREE.MeshFaceMaterial(materials));
scene.add(cube);
```


### 材质加载
```javascript
// 模型不含材质
var loader = new THREE.OBJLoader();
loader.load('../lib/port.obj', function(obj) {
    obj.traverse(function(child) {
        if (child instanceof THREE.Mesh) {
            // 开启双面绘制
            child.material.side = THREE.DoubleSide;
        }
    });

    mesh = obj;
    scene.add(obj);
});

// 模型含材质
var loader = new THREE.OBJLoader();
loader.load('../lib/port.obj', function(obj) {
    obj.traverse(function(child) {
        if (child instanceof THREE.Mesh) {
            child.material = new THREE.MeshLambertMaterial({
                color: 0xffff00,
                side: THREE.DoubleSide
            });
        }
    });

    mesh = obj;
    scene.add(obj);
});

//建模软件中设置材质
var loader = new THREE.OBJMTLLoader();
loader.addEventListener('load', function(event) {
    var obj = event.content;
    mesh = obj;
    scene.add(obj);
});
loader.load('../lib/port.obj', '../lib/port.mtl');
```

### 光源
```javascript
// 环境光
THREE.AmbientLight(hex)

// 点光源
THREE.PointLight(hex, intensity, distance)

// 平行光
new THREE.DirectionalLight();

// 聚光灯(hex, intensity, distance, angle, exponent)
var light = new THREE.SpotLight(0xffff00, 1, 100, Math.PI / 6, 25);

```

### 阴影
能形成阴影的光源只有THREE.DirectionalLight与THREE.SpotLight
能表现阴影效果的材质只有THREE.LambertMaterial与THREE.PhongMaterial

```javascript
// 告诉渲染器渲染阴影
renderer.shadowMapEnabled = true;

// 光源以及所有要产生阴影的物体调用
obj.castShadow = true;

// 接收阴影的物体调用
obj.receiveShadow = true;

// 调试时看到阴影相机
light.shadowCameraVisible = true;
```

阴影完整代码演示

```javascript
renderer = new THREE.WebGLRenderer();
renderer.shadowMapEnabled = true;
renderer.shadowMapSoft = true;

var plane = new THREE.Mesh(new THREE.PlaneGeometry(8, 8, 16, 16),
        new THREE.MeshLambertMaterial({color: 0xcccccc}));
plane.rotation.x = -Math.PI / 2;
plane.position.y = -1;
plane.receiveShadow = true;
scene.add(plane);

cube = new THREE.Mesh(new THREE.CubeGeometry(1, 1, 1),
        new THREE.MeshLambertMaterial({color: 0x00ff00}));
cube.position.x = 2;
cube.castShadow = true;
scene.add(cube);

var light = new THREE.SpotLight(0xffff00, 1, 100, Math.PI / 6, 25);
light.position.set(2, 5, 3);
light.target = cube;
light.castShadow = true;

// 聚光灯必须设置以下三值
light.shadowCameraNear = 2;
light.shadowCameraFar = 10;
light.shadowCameraFov = 30;
light.shadowCameraVisible = true;

light.shadowMapWidth = 1024;
light.shadowMapHeight = 1024;
light.shadowDarkness = 0.3;

scene.add(light);
```

### 着色器
使用着色器可以更加灵活地控制渲染效果，结合纹理，可以进行多次渲染，达到更加强大的效果
#### 着色器种类
1. 几何着色器(Geometry Shader,webgl基于OpenGL ES2.0，不支持该类型)
2. 顶点着色器(Veretx Shader)
   顶点着色器只调用一次，可以修改顶点位置与颜色传入片元着色器
3. 片元着色器(Fragment Shader)
   形成像素之前的数据，每个片元会调用一次，适合做图像后处理

#### 限定符
如果不写以下限定符，那么只能在当前文件可访问
1. const：常量
2. attribute:js代码传递到顶点着色器中，每个顶点对应不同的值
3. uniform:每个顶点/片元对应相同的值
4. varying: 从顶点着色器传递到片元着色器

看看着色器代码

```javascipt
// 申明一个叫vUV的变量，类型是vec2，为了将顶点着色器信息传递到片元着色器
varying vec2 vUv;

void main()
{
    /*
    * uv是threejs传进来代表UV映射时的横纵坐标
    * uv映射:将每个面片贴的图统一映射到一张纹理上,记录每个面片贴图在纹理上对应的位置
    */
    vUv = uv;
    /*
    * 计算三维模型在二维显示屏上的坐标
    * position是顶点在物体坐标系（而不是世界坐标系）中的位置
    */
    gl_Position = projectionMatrix * modelViewMatrix * vec4(position, 1.0);
}

varying vec2 vUv;
void main() {
    /*
    * vec4的四个维度分别表示红、绿、蓝以及alpha通道。
    * 因此，这里我们是将vUv的两个维度分别对应到红绿通道
    */
    gl_FragColor = vec4(vUv.x, vUv.y, 1.0, 1.0);
}
```


#### 着色器完整实例
```javascript
// 异步加载着色器代码
$.get('shader/my.vs', function(vShader){
    $.get('shader/my.fs', function(fShader){
         material = new THREE.ShaderMaterial({
            vertexShader: vShader,
            fragementShader: fShader
        });
    });
});

// HTML中加载
// <script id="vs" type="x-shader/x-vertex">
//     这里的内容相当于.vs文件中的内容
// </script>
// <script id="fs" type="x-shader/x-fragment">
//    这里的内容相当于.fs文件中的内容
// </script>
material = new THREE.ShaderMaterial({
    vertexShader: document.getElementById('vs').textContent,
    fragmentShader: document.getElementById('fs').textContent
});

// 程序
var scene = null;
var camera = null;
var renderer = null;
var cube = null;

function init() {
    renderer = new THREE.WebGLRenderer({
        canvas: document.getElementById('mainCanvas')
    });
    scene = new THREE.Scene();

    camera = new THREE.OrthographicCamera(-5, 5, 3.75, -3.75, 0.1, 100);
    camera.position.set(5, 15, 25);
    camera.lookAt(new THREE.Vector3(0, 0, 0));
    scene.add(camera);

    var light = new THREE.DirectionalLight();
    light.position.set(3, 2, 5);
    scene.add(light);

    cube = new THREE.Mesh(new THREE.CubeGeometry(2, 2, 2),
            new THREE.MeshLambertMaterial({color: 0x00ff00}));
    scene.add(cube);

    draw();
}

function draw() {
    cube.rotation.y += 0.01;
    if (cube.rotation.y > Math.PI * 2) {
        cube.rotation.y -= Math.PI * 2;
    }

    renderer.render(scene, camera);

    requestAnimationFrame(draw);
}
```


