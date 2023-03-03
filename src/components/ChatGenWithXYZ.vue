<template>
  <div ref="container" id="container"></div>
</template>

<script>
import * as THREE from 'three'
import {OrbitControls} from 'three/examples/jsm/controls/OrbitControls.js'
import {FontLoader} from 'three/examples/jsm/loaders/FontLoader.js'
import {TextGeometry} from 'three/examples/jsm/geometries/TextGeometry.js';

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

      const geometry3 = new THREE.TorusKnotGeometry(3, 0.2, 12, 8);
      const material3 = new THREE.MeshNormalMaterial({color: 0x00ff00});
      this.cube3 = new THREE.Mesh(geometry3, material3);
      this.cube3.position.set(-4, -4, -4)
      this.scene.add(this.cube3);

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

      // 添加半球光源
      // const hemisphereLight = new THREE.HemisphereLight(0xfeffff, 0x000000, 0.6);
      // this.scene.add(hemisphereLight);
      // 添加平行光
      // const directionalLight = new THREE.DirectionalLight(0xffffff, 1);
      // directionalLight.position.set(50, 40, 30); // 设置光源位置
      // directionalLight.target.position.set(10, 10, 10); // 设置光源目标位置
      // this.scene.add(directionalLight); // 将光源添加到场景中
      // 添加环境光助手
      // const ambientLightHelper = new THREE.AmbientLightHelper(this.ambientLight)
      // this.scene.add(ambientLightHelper)
      // 添加点光源助手
      // const pointLightHelper = new THREE.PointLightHelper(this.pointLight)
      // this.scene.add(pointLightHelper)
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
