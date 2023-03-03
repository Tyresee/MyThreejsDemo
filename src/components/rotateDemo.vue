<template>
  <div style="width: 100%; height: 100%">
    <div ref="canvasWrapper" style="width: 100%; height: 100vh"></div>
    <button @click="rotateCube" style="position: fixed;z-index: 100;top: 20px">Rotate</button>
  </div>

</template>

<script>
import * as THREE from 'three'
import TWEEN from '@tweenjs/tween.js'

export default {
  name: 'ThreeCube',
  data() {
    return {
      cube: null,
    }
  },
  mounted() {
    this.initThree()
  },
  methods: {
    initThree() {
      const width = this.$refs.canvasWrapper.clientWidth
      const height = this.$refs.canvasWrapper.clientHeight

      // 创建场景
      const scene = new THREE.Scene()

      // 创建相机
      const camera = new THREE.PerspectiveCamera(75, width / height, 0.1, 1000)
      camera.position.z = 5

      // 创建渲染器
      const renderer = new THREE.WebGLRenderer({ antialias: true })
      renderer.setSize(width, height)
      this.$refs.canvasWrapper.appendChild(renderer.domElement)

      // 创建立方体
      const geometry = new THREE.BoxGeometry()
      const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 })
      const cube = new THREE.Mesh(geometry, material)
      scene.add(cube)
      this.cube = cube

      // 创建环境光
      const ambientLight = new THREE.AmbientLight(0xffffff, 0.5)
      scene.add(ambientLight)

      // 创建平行光
      const directionalLight = new THREE.DirectionalLight(0xffffff, 0.5)
      directionalLight.position.set(1, 1, 1)
      scene.add(directionalLight)

      // 渲染场景
      const render = () => {
        requestAnimationFrame(render)
        TWEEN.update()
        renderer.render(scene, camera)
      }
      render()
    },
    rotateCube() {
      const rotation = { y: this.cube.rotation.y }
      const tween = new TWEEN.Tween(rotation)
          .to({ y: rotation.y + Math.PI / 12 }, 200)
          .onUpdate(() => {
            this.cube.rotation.y = rotation.y
          })
          .start()
      console.log(tween)
    },
  },
}
</script>

<style>
canvas {
  width: 100%;
  height: 100%;
  display: block;
}
</style>
