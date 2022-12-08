<template>
  <div class="main-page" ref="mainPage">
  </div>
</template>

<script setup lang="ts">
/**
*@file App
**/

import { onMounted, ref } from 'vue';
import * as THREE from 'three';
import { Mesh } from 'three';
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls';

const SIZE = {
  width: 800,
  height: 600
};
const SCENE = new THREE.Scene();
const CAMERA = new THREE.PerspectiveCamera(60, SIZE.width / SIZE.height, 0.1, 1000);
const RENDERER = new THREE.WebGL1Renderer();
const CLOCK = new THREE.Clock();
let ORBIT_CONSTROL: OrbitControls;

// axes helper
// const AXES_HELPER = new THREE.AxesHelper(100);
// SCENE.add(AXES_HELPER);

RENDERER.setSize(SIZE.width, SIZE.height);

// main-page ref
let mainPage = ref<HTMLDivElement>();

onMounted(() => {
  mainPage.value?.appendChild(RENDERER.domElement);
  let cube = drawACube();  // 在这之前不会出现任何图形，页面保持空白

  // Control
  ORBIT_CONSTROL = new OrbitControls(CAMERA, mainPage.value?.childNodes[0] as HTMLElement);
  ORBIT_CONSTROL.enableDamping = true;

  animate(cube)();
});

// const cursorPosition = {
//   x: 0,
//   y: 0
// };
// window.addEventListener('mousemove', (event: MouseEvent) => {
//   cursorPosition.x = event.clientX;
//   cursorPosition.y = event.clientY;
// })

const drawACube = (): Mesh => {
  const geometry = new THREE.BoxGeometry(1, 1, 1, 5, 5, 5);
  const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
  const cubeMesh: Mesh = new THREE.Mesh(geometry, material);

  SCENE.add(cubeMesh);
  CAMERA.position.z = 5;
  return cubeMesh;
};

const animate = (cubeMesh: Mesh) => {
  return () => {
    let elapsedTime = CLOCK.getElapsedTime();
    // console.log(elapsedTime);

    // 正方体的正面会跟着你的鼠标改变朝向
    // cubeMesh.rotation.x = cursorPosition.y / SIZE.height - .5;
    // cubeMesh.rotation.y = cursorPosition.x / SIZE.width - .5;

    // CAMERA.position.x = -(cursorPosition.x / SIZE.width - .5) * 10;
    // CAMERA.position.y = (cursorPosition.y / SIZE.height - .5) * 10;
    // CAMERA.lookAt(new THREE.Vector3());

    ORBIT_CONSTROL.update();

    RENDERER.render(SCENE, CAMERA);

    requestAnimationFrame(animate(cubeMesh));
  };
};

</script>

<style lang="scss">
.main-page {
  width: 100%;

  .main-page_bg {
    width: 100%;
  }
}
</style>
