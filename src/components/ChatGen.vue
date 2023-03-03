<template>
  <div ref="container"></div>
</template>
<script>
import * as THREE from 'three'
import {OrbitControls} from 'three/examples/jsm/controls/OrbitControls.js'

export default {
  name: 'chat-gen',
  data() {
    return {
      scene: null,
      camera: null,
      renderer: null,
      controls: null,
      cube1: null,
      cube2: null,
      axesHelper: null,
    }
  },
  mounted() {
    this.initScene()
    this.initCamera()
    this.initRenderer()
    this.initControls()
    this.initObjects()
    this.initAxesHelper()
    this.animate()
  },
  beforeUnmount() {
    cancelAnimationFrame(this.animationFrameId)
    this.controls.dispose()
    this.renderer.dispose()
    this.scene.dispose()
  },
  methods: {
    initScene() {
      this.scene = new THREE.Scene()
    },
    initCamera() {
      this.camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000)
      this.camera.position.set(0, 0, 5)
    },
    initRenderer() {
      this.renderer = new THREE.WebGLRenderer({antialias: true})
      this.renderer.setSize(window.innerWidth, window.innerHeight)
      this.renderer.setClearColor(0x000000)
      this.$refs.container.appendChild(this.renderer.domElement)
    },
    initControls() {
      this.controls = new OrbitControls(this.camera, this.renderer.domElement)
    },
    initObjects() {
      const geometry1 = new THREE.BoxGeometry(1, 1, 1)
      const material1 = new THREE.MeshBasicMaterial({color: 0xff0000})
      this.cube1 = new THREE.Mesh(geometry1, material1)
      this.scene.add(this.cube1)

      const geometry2 = new THREE.BoxGeometry(0.5, 0.5, 0.5)
      const material2 = new THREE.MeshBasicMaterial({color: 0x00ff00})
      this.cube2 = new THREE.Mesh(geometry2, material2)
      this.cube2.position.x = 1.5
      this.scene.add(this.cube2)
    },
    initAxesHelper() {
      this.axesHelper = new THREE.AxesHelper(2)
      this.scene.add(this.axesHelper)
    },
    animate() {
      this.animationFrameId = requestAnimationFrame(this.animate)
      this.controls.update()
      this.renderer.render(this.scene, this.camera)
    },
    
  }
}
</script>
<style>
#container {
  width: 100%;
  height: 100%;
}
</style>
