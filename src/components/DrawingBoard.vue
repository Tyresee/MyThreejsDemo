<template>
  <div class="wrapper">
    <div class="info" style="position: fixed;top: 0;z-index: 1000;color: pink">
      <div class="pink">
        camera{{ camera }}
      </div>
      <br>
      <div class="yellow">
        scene {{ scene }}
      </div>
      <br>
    </div>
    <div class="main">


    </div>
  </div>

</template>

<script>
import * as THREE from 'three'
// import {OrbitControls} from "three/examples/jsm/controls/OrbitControls"
// import {OrbitControls} from './OrbitControls'
import {OrbitControls} from 'three-orbitcontrols';

export default {
  name: 'DrawingBoard',
  props: {},
  data() {
    return {
      camera: {},
      scene: {},
      geometry: {},
      material: {},
      controls: {},
      meshList: []
    }
  },
  methods: {
    animation(time) {
      this.mesh.rotation.x = time / 2000
      this.mesh.rotation.y = time / 2000
      this.renderer.render(this.scene, this.camera)
    },
    render() {
      this.renderer.render(this.scene, this.camera)
      requestAnimationFrame(this.render)
    },
    setCamera() {
      this.camera = new THREE.PerspectiveCamera(90, window.innerWidth / window.innerHeight, .01, 10)
      this.camera.position.set(0, 1.5, 5)

    },
    setScene() {
      this.scene = new THREE.Scene()
      this.meshList.forEach(i => {
        this.scene.add(i)
      })


    },
    setMaterial() {
      this.material = new THREE.MeshNormalMaterial()
    },
    setGeometry() {
      this.geometry = new THREE.BoxGeometry(1, 0.5, 1)
    },
    setLight() {
      this.light = new THREE.DirectionalLight('pink', 10)
      this.light.position.set(0, 10, 0)
      this.light.target.position.set(10, 0, 0)
    },
    setMesh() {
      const mesh1 = new THREE.Mesh(new THREE.BoxGeometry(1, 0.5, 1), this.material)
      mesh1.position.set(1, 1, 0)

      const mesh2 = new THREE.Mesh(new THREE.CircleGeometry(), this.material)
      mesh2.position.set(0, 2, 0)
      mesh2.rotation.set(0, 1, 0)

      this.meshList.push(mesh1, mesh2)
    },
    setRenderer() {
      this.renderer = new THREE.WebGLRenderer({antialias: true})
      this.renderer.setSize(this.drawingDom.clientWidth, this.drawingDom.clientHeight)
    },
    setControls() {
      this.controls = new OrbitControls(this.camera, this.drawingDom)
    }
  },
  mounted() {
    this.drawingDom = document.querySelector('.main')
    this.setCamera()
    this.setGeometry()
    this.setMaterial()
    this.setMesh()
    this.setLight()
    this.setScene()
    this.setRenderer()
    // this.renderer.setAnimationLoop(this.animation)
    this.setControls()
    this.render()

    this.drawingDom.appendChild(this.renderer.domElement);
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.main {
  width: 100vw;
  height: 100vh;
  /*background: pink;*/
}

.pink {
  color: pink;
}

.yellow {
  color: yellow;
}

</style>
