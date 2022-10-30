<template>
  <div>
    <div ref="canvas"></div>

    canvas
  </div>
</template>

<script setup>
import { ref, onMounted, reactive, computed, render } from 'vue'
import * as THREE from "three"
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js'

const canvas = ref(null)

onMounted(() => {
  canvas.value.appendChild( renderer.domElement )
})

const props = defineProps({
  geometry: { type: String, default: 'torus' },
  color: { type: String, default: 'black' },
})

// Constants
const geometries = {
  torus: {
    method: 'TorusGeometry',
    args: [1.5, 0.4, 16, 100]
  },
  box: {
    method: 'BoxGeometry',
    args: [1, 1, 1]
  }
}

const colors = {
  black: 0x000000,
  red: 0xE06666
}

const scene = new THREE.Scene()
const camera = new THREE.PerspectiveCamera( 70, 2, 1, 1000 )
const renderer = new THREE.WebGLRenderer()
const geometry = new THREE[geometries[props.geometry].method](...geometries[props.geometry].args)

// const material = new THREE.MeshBasicMaterial( { color: colors[props.color] } )
const texture = new THREE.TextureLoader().load('https://images.unsplash.com/photo-1604079628040-94301bb21b91?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=2731&q=80');
const material = new THREE.MeshBasicMaterial( { map: texture } );

const figure = new THREE.Mesh( geometry, material )
const controls = new OrbitControls(camera, renderer.domElement)

scene.add( figure )

// Renderer Settings
renderer.setClearColor( 0xffffff, 0)

// Camera Settings
camera.position.z = 5

// Controls Settings
controls.enableZoom = false
controls.update()

function animate() {
  resizeCanvasToDisplaySize()

  // Animation
  figure.rotation.x += 0.01
  figure.rotation.y += 0.01

  controls.update()

  renderer.render( scene, camera )
  
  requestAnimationFrame( animate )
}

function resizeCanvasToDisplaySize() {
  const canvas = renderer.domElement
  const width = canvas.clientWidth
  const height = canvas.clientHeight

  if (canvas.width !== width || canvas.height !== height) {
    renderer.setSize(width, height, false)
    camera.aspect = width / height
    camera.updateProjectionMatrix()
  }
}

animate()
</script>

<style lang="scss">
canvas {
  width: 100%;
  height: 600px;
}
</style>