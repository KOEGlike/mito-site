<script lang="ts">
  import { onMount } from "svelte";
  import * as THREE from "three";
  import { OrbitControls } from "three/addons/controls/OrbitControls.js";
  import { GLTFLoader, type GLTF } from 'three/addons/loaders/GLTFLoader.js';
  let container: HTMLDivElement;

  onMount(() => {
    const scene = new THREE.Scene();
    const camera = new THREE.PerspectiveCamera(
      75,
      window.innerWidth / window.innerHeight,
      0.1,
      1000
    );

    const ambientLight = new THREE.AmbientLight(0xffffff, 1.2);
    scene.add(ambientLight);

    //const dirLight = new THREE.DirectionalLight(0xffffff, 2.0);
    //dirLight.position.set(200, 200, 200);
    //scene.add(dirLight);

    const renderer = new THREE.WebGLRenderer();
    renderer.setSize(window.innerWidth, window.innerHeight);
    container.appendChild(renderer.domElement);

    let controls = new OrbitControls(camera, renderer.domElement);
    controls.minDistance = 1;
    controls.maxDistance = 200;
    controls.enableDamping = true;
    controls.autoRotate=true;
    controls.autoRotateSpeed=0.5;

    let loader = new GLTFLoader();
    let pcb:GLTF;
    loader.load("mito-pcb.glb", function (object) {
      pcb=object;
      scene.add(pcb.scene);
      controls.reset();
    });

    camera.position.z = 5;

    function animate() {
      controls.update();
      renderer.render(scene, camera);
    }
    renderer.setAnimationLoop(animate);
  });
</script>

<div bind:this={container}></div>

<style>
  div {
    width: 100%;
    height: 100%;
  }
</style>
