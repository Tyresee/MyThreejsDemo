<template>
  <div style="width: 100vw; height: 100vh">
    <div ref="container" id="container"></div>
    <button class="control-btn" @click="rotateCube('y', 'add')" style="top: 20px">RotateY+</button>
    <button class="control-btn" @click="rotateCube('y', 'reduce')" style="top: 40px">RotateY-</button>
    <button class="control-btn" @click="rotateCube('x', 'add')" style="top: 80px">RotateX+</button>
    <button class="control-btn" @click="rotateCube('x', 'reduce')" style="top: 100px">RotateX-</button>
    <button class="control-btn" @click="rotateCube('z', 'add')" style="top: 140px">RotateZ+</button>
    <button class="control-btn" @click="rotateCube('z', 'reduce')" style="top: 160px">RotateZ-</button>


    <button class="control-btn" @click="translateCube('x', 'add')" style="top: 200px">TranslateY+</button>
    <button class="control-btn" @click="translateCube('x', 'reduce')" style="top: 220px">TranslateY-</button>
    <button class="control-btn" @click="translateCube('y', 'add')" style="top: 260px">TranslateX+</button>
    <button class="control-btn" @click="translateCube('y', 'reduce')" style="top: 280px">TranslateX-</button>
    <button class="control-btn" @click="translateCube('z', 'add')" style="top: 320px">TranslateZ+</button>
    <button class="control-btn" @click="translateCube('z', 'reduce')" style="top: 340px">TranslateZ-</button>
  </div>

</template>

<script>
import * as THREE from 'three'
import {OrbitControls} from 'three/examples/jsm/controls/OrbitControls.js'
import {FontLoader} from 'three/examples/jsm/loaders/FontLoader.js'
import {TextGeometry} from 'three/examples/jsm/geometries/TextGeometry.js';
import TWEEN from '@tweenjs/tween.js'


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
      font: null,
      xAxis: null,
      yAxis: null,
      zAxis: null
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
      // this.scene.background = new THREE.Color(0xefedff);
    },
    initCamera() {
      this.camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 100)
      this.camera.position.set(3, 3, 3)
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
      console.log(new THREE.Color().setStyle('pink'), 'color')
      // const material1 = new THREE.MeshLambertMaterial({color: 0x203c31})
      const material1 = new THREE.MeshNormalMaterial({color: new THREE.Color().setStyle('green')})
      this.cube1 = new THREE.Mesh(geometry1, material1)
      this.cube1.castShadow = true;
      this.cube1.receiveShadow = true;
      this.scene.add(this.cube1)

      const geometry2 = new THREE.BoxGeometry(0.5, 0.3, 0.5)
      const material2 = new THREE.MeshNormalMaterial({color: 0xf00f0f})
      this.cube2 = new THREE.Mesh(geometry2, material2)
      this.cube2.position.set(1.5, .3, .3)
      this.scene.add(this.cube2)


      const fontLoader = new FontLoader()
      fontLoader.load('/fonts/typeface.json', (font) => {
        this.font = font
        this.initAxis()
      })
    },
    initAxis() {
      const fontLoader = new FontLoader()
      fontLoader.load("/fonts/typeface.json", (font) => {
        const axisGroup = new THREE.Group()

        const options = {
          font,
          size: 0.2,
          height: 0.02,
          curveSegments: 12,
          bevelEnabled: false,
          bevelThickness: 0.01,
          bevelSize: 0.01,
          bevelOffset: 0,
          bevelSegments: 5
        }

        // x axis
        const xGeometry = new TextGeometry('X', options)
        const xMaterial = new THREE.MeshBasicMaterial({color: 0xff0000})
        const xMesh = new THREE.Mesh(xGeometry, xMaterial)
        xMesh.position.x = 1
        axisGroup.add(xMesh)

        // y axis
        const yGeometry = new TextGeometry('Y', options)
        const yMaterial = new THREE.MeshBasicMaterial({color: 0x00ff00})
        const yMesh = new THREE.Mesh(yGeometry, yMaterial)
        yMesh.position.y = 1
        axisGroup.add(yMesh)

        // z axis
        const zGeometry = new TextGeometry('Z', options)
        const zMaterial = new THREE.MeshBasicMaterial({color: 0x0000ff})
        const zMesh = new THREE.Mesh(zGeometry, zMaterial)
        zMesh.position.z = 1
        axisGroup.add(zMesh)

        this.scene.add(axisGroup)
      })
    },
    initLighting() {
      // 添加环境光
      this.ambientLight = new THREE.AmbientLight(0xfffff, 13)
      this.scene.add(this.ambientLight)

      // 添加点光源
      this.pointLight = new THREE.PointLight(0xfffff, 1, 10)
      this.pointLight.position.set(10, 10, 10)
      this.scene.add(this.pointLight)

    },

    animate() {
      this.animationFrameId = requestAnimationFrame(this.animate)
      this.controls.update()
      TWEEN.update()
      this.renderer.render(this.scene, this.camera)
    },
    rotateCube(axis, direction) {
      const rotation = { [axis]: this.cube2.rotation[axis] }
      const step = direction === 'add' ? Math.PI / 12 : Math.PI / 12 * (-1)
      new TWEEN.Tween(rotation)
          .to({ [axis]: rotation[axis] + step }, 200)
          .onUpdate(() => {
            this.cube2.rotation[axis] = rotation[axis]
          })
          .start()
    },

    translateCube(axis,direction) {
      const position = {[axis]: this.cube2.position[axis]}
      const step = direction === 'add' ? Math.PI / 12 : Math.PI / 12 * (-1)
      new TWEEN.Tween(position)
          .to({[axis]: position[axis] + step}, 200)
          .onUpdate(() => {
            this.cube2.position[axis] = position[axis]
          })
          .start()
    },
  }
}

</script>
<style>
#container {
  width: 100%;
  height: 100%;
}
.control-btn {
  position: fixed;
  z-index: 100;
}
</style>
