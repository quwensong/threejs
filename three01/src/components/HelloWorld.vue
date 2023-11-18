<template>
  <div class="hello" ref="hello"></div>
</template>

<script>
import * as THREE from "three";
// 引入Three.js扩展库
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls.js";

export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  mounted() {
    // 创建场景
    var scene = new THREE.Scene();
    // 创建相机PerspectiveCamera（视野角度,长宽比,近截面（near）和远截面（far)） 透视相机
    var camera = new THREE.PerspectiveCamera(
      75,
      window.innerWidth / window.innerHeight,
      0.1,
      1000
    );
    camera.position.z = 5; //将摄像机稍微向外移动一些
    // 创建立方体
    const geometry = new THREE.BoxGeometry(); //立方体对象，包括顶点、面
    const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 }); //MeshBasicMaterial立方体的材质
    const cube = new THREE.Mesh(geometry, material); //网格，包括一个几何体以及作用在此几何体上的材质
    // 创建渲染器
    const renderer = new THREE.WebGLRenderer();
    renderer.setSize(window.innerWidth, window.innerHeight);
    // document.body.appendChild( renderer.domElement );
    this.$refs.hello.appendChild(renderer.domElement);
    scene.add(cube); //将物体添加到(0,0,0)坐标，的网格上
    // 渲染函数
    function animate() {
      requestAnimationFrame(animate);
      cube.rotation.x += 0.01;
      cube.rotation.y += 0.01;
      renderer.render(scene, camera);
    }
    animate();
  },
};
</script>
