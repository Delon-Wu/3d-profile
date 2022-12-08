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
const CLOCK = new THREE.Clock();

// axes helper
const AXES_HELPER = new THREE.AxesHelper(100);
SCENE.add(AXES_HELPER);

RENDERER.setSize(window.innerWidth, window.innerHeight);

// main-page ref
let mainPage = ref<HTMLDivElement>();

onMounted(() => {
  mainPage.value?.appendChild(RENDERER.domElement);
  let cube = drawACube();  // 在这之前不会出现任何图形，页面保持空白
  cube.scale.x = 2;  // 沿x轴放大x倍

  // cube.position.x = .7;
  // cube.position.y = -.6;
  // cube.position.z = 1;
  cube.position.set(2, -.6, 1);

  // Rotation
  // cube.rotation.x = Math.PI / 4;
  // cube.rotation.y = Math.PI / 4;
  // cube.rotation.z = Math.PI / 3;
  // cube.rotation.reorder('XYZ')

  // Change camera
  // CAMERA.lookAt(cube.position);
  // CAMERA.position.x = 1

  animate(cube)();
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

const animate = (cubeMesh: Mesh) => {
  return () => {
    let elapsedTime = CLOCK.getElapsedTime();
    // console.log(elapsedTime);

    // cubeMesh.position.y = Math.sin(elapsedTime);  // the cube flow up like magic

    RENDERER.render(SCENE, CAMERA);

    requestAnimationFrame(animate(cubeMesh));
  };
};

const initMouseEvent = (cubeMesh: Mesh) => {
  document.body.addEventListener('click', (e) => {
    cubeMesh.rotation.x += .1;
    cubeMesh.rotation.y += .1;
  });
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
