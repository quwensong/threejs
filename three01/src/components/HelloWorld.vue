<template>
  <div class="hello" ref="hello"></div>
</template>

<script>
import dat from 'dat.gui'
import * as THREE from "three";
// 引入Three.js扩展库 控制器
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls.js";

export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  mounted() {
      // 创建控制对象
      const controlData = {
        rotationSpeed: 0.01,
        color: "#66ccff",
        wireframe: false, //线框
        envMap: "无",
      };


      // 创建dat实例对象
      const gui = new dat.GUI();

      const config = gui.addFolder("配置");
      // config.add(controlData,"rotationSpeed",0.01,0.1,0.01);
      // 或者下面这种
      config.add(controlData, "rotationSpeed").min(0.01).max(0.1).step(0.01);
      // 颜色选择器
      // const color = gui.addFolder("颜色");
      config.addColor(controlData, "color") ;
      // 下拉列表
      config.add(controlData, "envMap",['无','全反射','漫反射']);
      // true false
      config.add(controlData, "wireframe" );

      config.domElement.id = 'gui'
      config.open();


    // 创建场景
    const scene = new THREE.Scene();
    // 创建相机PerspectiveCamera（视野角度,长宽比,近截面（near）和远截面（far)） 透视相机
    const camera = new THREE.PerspectiveCamera(
      75,
      window.innerWidth / window.innerHeight,
      0.1,
      1000
    );
    camera.position.z = 3; //将摄像机稍微向外移动一些
    camera.position.y = 3; //将摄像机稍微向外移动一些
    camera.position.x = 3; //将摄像机稍微向外移动一些
    camera.lookAt(0, 0, 0);
    // 创建立方体
    const geometry = new THREE.BoxGeometry(); //立方体对象，包括顶点、面
    const material = new THREE.MeshBasicMaterial({ color:controlData.color}); //MeshBasicMaterial立方体的材质
    const cube = new THREE.Mesh(geometry, material); //网格，包括一个几何体以及作用在此几何体上的材质;
    // 创建渲染器
    const renderer = new THREE.WebGLRenderer();
    renderer.setSize(window.innerWidth, window.innerHeight);

    // 添加坐标辅助器
    const axesHelper = new THREE.AxesHelper(5);
    axesHelper.position.set(0,0.2,0)
    // 添加网格地面
    const gridHelper = new THREE.GridHelper(9, 9);

    // 添加轨道控制器
    const controls = new OrbitControls(camera, renderer.domElement);

    this.$refs.hello.appendChild(config.domElement);

    this.$refs.hello.appendChild(renderer.domElement);

    
    scene.add(cube); //将物体添加到(0,0,0)坐标，的网格上
    scene.add(axesHelper);
    scene.add(gridHelper);   




    // 渲染函数
    function animate() {
      controls.update();
      //  requestAnimationFrame 会自动随着屏幕的刷新频率自动执行
      requestAnimationFrame(animate);
      cube.rotation.x += controlData.rotationSpeed;
      cube.rotation.y += controlData.rotationSpeed;
      cube.material.color = new THREE.Color(controlData.color);
      material.wireframe = controlData.wireframe

      renderer.render(scene, camera);
    }
    animate();
  },
};
</script>

<style>

#gui{
  position: absolute;
  right: 0;
  width: 300px;
}
</style>
