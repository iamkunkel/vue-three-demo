<template>
  <div class="container">
    <div class="page-des">3D旋转盒子模型</div>
    <div
      id="rotateBox"
      class="three-area"
    ></div>
  </div>

</template>

<script>
import * as THREE from 'three'
import Stats from 'three/examples/jsm/libs/stats.module.js' // JavaScript性能监视器
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls"

export default {
  name: 'rotateBox',
  data() {
    return {
      camera: null,
      scene: null,
      renderer: null,
      mesh: null,
      controls: null,
      stats: null,
    }
  },
  mounted() {
    this.init()
    this.render()
  },
  beforeDestroy() {
    if (this.camera) this.camera = null
    if (this.scene) this.scene = null
    if (this.renderer) this.renderer = null
    if (this.mesh) this.mesh = null
    if (this.controls) this.controls = null
    if (this.stats) this.stats = null
  },
  methods: {
    init() {
      let container = document.getElementById('rotateBox')
      this.stats = new Stats() // JavaScript性能监视器
      this.stats.domElement.style.position = 'absolute'
      this.stats.domElement.style.top = '50px'
      this.stats.showPanel(1); // 0: fps, 1: ms, 2: mb, 3+: custom
      container.appendChild(this.stats.domElement)
      /**
       *  相机的lookAt 方向 与相机 up方向必须垂直
       *  已知三维向量 a = (x1,y1,z1)
       *  已知三维向量 b = (x2,y2,z2)
       *  向量A与向量B垂直则表示为： x1*x2 + y1*y2 + z1*z2 = 0
       */
      this.camera = new THREE.PerspectiveCamera(75, container.clientWidth / container.clientHeight, 1, 1000)
      this.camera.position.z = 3 // 相机位置
      this.camera.position.y = 3
      this.camera.position.x = 3
      this.camera.up.x = 0
      this.camera.up.y = 1  // 设置相机快门的方向
      this.camera.up.z = 0
      this.camera.lookAt(0, 0, 0) // 相机视点方向

      this.scene = new THREE.Scene()
      let geometry = new THREE.BoxGeometry(1, 1, 1)
      let material = new THREE.MeshNormalMaterial()
      this.mesh = new THREE.Mesh(geometry, material)
      // this.mesh.position.set(1,1,1)
      this.scene.add(this.mesh)

      const axeshelper = new THREE.AxesHelper(5)
      this.scene.add(axeshelper)

      this.renderer = new THREE.WebGLRenderer({ antialias: true })
      this.renderer.setPixelRatio(window.devicePixelRatio); //设置canvas的像素比为当前设备的屏幕像素比，避免高分屏下模糊
      this.renderer.setSize(window.innerWidth, window.innerHeight)
      this.renderer.setClearColor(0xb9d3ff, 1)
      container.appendChild(this.renderer.domElement)
      this.controls = new OrbitControls(this.camera, this.renderer.domElement)
    },

    render() {
      this.stats.begin()
      this.mesh.rotation.y += 0.01
      this.renderer.clear()
      this.renderer.render(this.scene, this.camera)
      this.stats.end()
      requestAnimationFrame(this.render)
    },

  }

}

</script>
