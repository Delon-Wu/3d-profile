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

const SCENE = new THREE.Scene();
const CAMERA = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
const RENDERER = new THREE.WebGL1Renderer();
RENDERER.setSize(window.innerWidth, window.innerHeight)

// main-page ref
let mainPage = ref<HTMLDivElement>();

onMounted(() => {
  mainPage.value?.appendChild(RENDERER.domElement);
  let cube = drawACube();  // 在这之前不会出现任何图形，页面保持空白

  animate();
  initMouseEvent(cube);
});

const drawACube = (): Mesh => {
  const geometry = new THREE.BoxGeometry(1, 1, 1);
  const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
  const cubeMesh: Mesh = new THREE.Mesh(geometry, material);

  SCENE.add(cubeMesh);
  CAMERA.position.z = 5;
  return cubeMesh;
};

const animate = () => {
  requestAnimationFrame(animate);
  RENDERER.render(SCENE, CAMERA);
};

const initMouseEvent = (cubeMesh: Mesh) => {
  document.body.addEventListener('click', (e) => {
    cubeMesh.rotation.x += .1;
    cubeMesh.rotation.y += .1;
  })
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
