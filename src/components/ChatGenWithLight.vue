<template>
  <div ref="container"></div>
</template>

<script>
import * as THREE from 'three'
import {OrbitControls} from 'three/examples/jsm/controls/OrbitControls.js'

export default {
  name: 'ThreeScene',
  data() {
    return {
      scene: null,
      camera: null,
      renderer: null,
      controls: null,
      cube: null,
      ambientLight: null,
      pointLight: null
    }
  },
  mounted() {
    this.initScene()
    this.initCamera()
    this.initRenderer()
    this.initControls()
    this.initObjects()
    this.initLighting()
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
      const geometry = new THREE.BoxGeometry(1, 1, 1)
      const material = new THREE.MeshPhongMaterial({color: 0xffffff})
      this.cube = new THREE.Mesh(geometry, material)
      this.scene.add(this.cube)
    },
    initLighting() {
      // 添加环境光
      this.ambientLight = new THREE.AmbientLight(0xffffff, 0.5)
      this.scene.add(this.ambientLight)

      // 添加点光源
      this.pointLight = new THREE.PointLight(0xffffff, 11, 100)
      this.pointLight.position.set(2, 4, 3)
      this.scene.add(this.pointLight)
    },
    animate() {
      this.animationFrameId = requestAnimationFrame(this.animate)
      this.controls.update()
      this.renderer.render(this.scene, this.camera)
    }
  }
}
</script>

<style>
#container {
  width: 100%;
  height: 100%;
}
</style>
