<template>
  <div class="scene">
    <div id="three-scene-canvas"></div>
  </div>
</template>

<script>
import * as THREE from 'three'
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls'
export default {
  name: 'Scene',
  data () {
    return {
      sceneCanvas: null,
      scene: null,
      camera: null,
      renderer: null,
      controls: null
    }
  },
  mounted () {
    this.initScene()
    window.addEventListener('resize', () => {this.windowResize()})
  },

  methods: {
    initScene(){
      /* **************
        BASIC SETUP
      ************** */
      this.sceneCanvas = document.getElementById('three-scene-canvas')
      this.scene = new THREE.Scene()
      this.camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight,0.1,1000)
      this.camera.position.set(15, 5, 15)
      
      // render
      this.renderer = new THREE.WebGLRenderer({
        antialias: true,
        powerPreference: "high-performance"
      })
      this.renderer.outputEncoding = THREE.sRGBEncoding
      this.controls = new OrbitControls(this.camera, this.renderer.domElement)
      this.renderer.setClearColor("#212121")
      this.renderer.setPixelRatio( window.devicePixelRatio );
      this.renderer.setSize(window.innerWidth, window.innerHeight);
      this.renderer.shadowMap.enabled = true
      this.renderer.shadowMap.type = THREE.PCFSoftShadowMap
      this.renderer.shadowMapSoft = true
      this.renderer.shadowMap.autoUpdate = false
      this.renderer.shadowMap.needsUpdate = true
      this.sceneCanvas.append(this.renderer.domElement)

      
      // light
      let ambientLight = new THREE.AmbientLight (0xdaccff, 0.5)
      this.scene.add(ambientLight)
      let light = new THREE.PointLight(0xfc831d, 1, 100)
      light.position.set(15, 10, 15)
      light.castShadow = true
      light.shadow.radius = 1
      light.shadow.mapSize.width = 2048
      light.shadow.mapSize.height = 2048
      this.scene.add(light)

      // cube
      let geometry = new THREE.BoxGeometry(10,10,10)
      let material = new THREE.MeshPhysicalMaterial({color: 0x00ff00})
      let cube = new THREE.Mesh(geometry, material)
      this.scene.add(cube)

      this.animateThreeJs()
    },
    
    // resize
    windowResize(){
      this.camera.aspect = window.innerWidth / window.innerHeight;
      this.camera.updateProjectionMatrix()
      this.renderer.setSize(window.innerWidth, window.innerHeight);
    },

    // animate
    animateThreeJs () {
      requestAnimationFrame(this.animateThreeJs)
      this.controls.update();
      this.renderer.render(this.scene, this.camera)
      this.renderer.shadowMap.needsUpdate = true
    },

  }
}

</script>

<style >
  #three-scene-canvas {
    width: 100%;
    height: 100vh;
    overflow: hidden;
  }
</style>