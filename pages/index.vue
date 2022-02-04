<template>
    <div>
    <canvas ref = "canvas"></canvas>
    <div id = "div" class ='absolute text-white text-center font-size: 1rem max-w-2xl px-6' style = "top: 50%;
    transform:translateY(-30%, -100%); left: 45%; top: 40%">
      <h1 style = "transform: translateY(30px)" class="text-center font-space-mono text-xl uppercase tracking-wide opacity-0" id="albert">Albert Yao</h1>
      <p style = "transform: translateY(30px)" class="text-center font-exo text-4xl uppercase opacity-0" id="pg1">Testing 123</p>
      <a style = "transform: translateY(30px)" id="bbb" href="https://freetechnologyhelp.com" class="text-center border px-6 py-3 rounded-lg font-space-mono uppercase mt-6 hover:bg-white hover:text-gray-800
       inline-block opacity-0">Start Guide</a>
    </div>
    </div>
</template>

<script>
import gsap from 'gsap'
import {PlaneGeometry, Raycaster, Scene, PerspectiveCamera, WebGLRenderer, MeshPhongMaterial, FlatShading, Mesh, BufferAttribute, DirectionalLight, BufferGeometry, PointsMaterial, Points, DoubleSide, Float32BufferAttribute} from 'three'

export default {
  mounted() {
      // Find the latest version by visiting https://cdn.skypack.dev/three.
      const world = {
        plane: {
          width: 400,
          height: 400,
          widthSegments: 50,
          heightSegments:50
        }
      }

      function generatePlane(){
        planeMesh.geometry.dispose()
        planeMesh.geometry = new PlaneGeometry(
          world.plane.width, world.plane.height, world.plane.widthSegments, world.plane.heightSegments
        )


        //vertice position random
      const{array} = planeMesh.geometry.attributes.
      position
      const randomValues = []
      for(let i = 0; i < array.length;i++){
        if(i % 3 === 0){
          const x = array[i]
          const y = array[i+1]
          const z = array[i+2]
          array[i] = x + Math.random() - 0.5
          array[i + 1] = y + (Math.random() - 0.5) * 6
          array[i + 2] = z + (Math.random() - 0.5) * 6

        }


        randomValues.push(Math.random() * Math.PI * 2)
      }
      console.log(planeMesh.geometry.attributes.position)
      planeMesh.geometry.attributes.position.randomValues = randomValues
      planeMesh.geometry.attributes.position.originalPosition = planeMesh.geometry.attributes.position.array
        var colors = []
        for(let i = 0; i < planeMesh.geometry.attributes.position.count; i++){
          colors.push(0, 0.19, 0.4)
        }
      planeMesh.geometry.setAttribute('color', 
        new BufferAttribute(
          new Float32Array(colors), 3))

      }
      const raycaster = new Raycaster()
      const scene = new Scene();
      const camera = new PerspectiveCamera(75, innerWidth / innerHeight, 
        0.1, 
        1000
      )
      const renderer = new WebGLRenderer({
      canvas: this.$refs.canvas
      })
      renderer.setSize(innerWidth, innerHeight)
      renderer.setPixelRatio(devicePixelRatio)
      camera.position.z = 60
      const planeGeometry = new PlaneGeometry(
        world.plane.width, world.plane.height, world.plane.widthSegments, world.plane.heightSegments
      )
      const planeMaterial = new MeshPhongMaterial({side: DoubleSide,
        flatShading: FlatShading,
      vertexColors:true})
      const planeMesh = new Mesh(planeGeometry, planeMaterial)
      var colors = []
      for(let i = 0; i < planeMesh.geometry.attributes.position.count; i++){
        colors.push(0, 0.19, 0.4)
      }
      generatePlane()
      planeMesh.geometry.setAttribute('color', 
        new BufferAttribute(
          new Float32Array(colors), 3))
      scene.add(planeMesh)

      const light = new DirectionalLight(
        0xffffff, 1
      )

      light.position.set(0, 1, 1)
      scene.add(light)

      const backLight = new DirectionalLight(
        0xffffff, 1
      )
      backLight.position.set(0, 0, -1)
      scene.add(backLight)

      const starGeometry = new BufferGeometry()
      const starMaterial = new PointsMaterial({
        color: 0xffffff
      })

      const starVerticies = []

      for(let i = 0; i < 3000; i++){
        const x = (Math.random() - 0.5) * 2000
        const y = (Math.random() - 0.5) * 2000
        const z = (Math.random() - 0.5) * 2000
        starVerticies.push(x, y, z)
      }

      starGeometry.setAttribute('position', new Float32BufferAttribute(
        starVerticies, 3
      ))



      const stars = new Points(starGeometry, starMaterial)

      scene.add(stars)
      let frame = 0
      function animate(){
        requestAnimationFrame(animate)
        frame += 0.01
        renderer.render(scene, camera)
        const { array, originalPosition, randomValues } = planeMesh.geometry.attributes.position
        for (let i = 0; i < array.length;i+= 3){
            array[i] = originalPosition[i] + Math.cos(frame + randomValues[i]) * 0.015
            array[i+1] = originalPosition[i+1] + Math.sin(frame + randomValues[i]) * 0.015
          }
        planeMesh.geometry.attributes.position.needsUpdate = true

        raycaster.setFromCamera(mouse, camera)
        const intersects = raycaster.intersectObject(planeMesh)
        if(intersects.length > 0){
          const {color} = intersects[0].object.geometry.attributes
          color.setX(intersects[0].face.a, 0.1)
          color.setX(intersects[0].face.b, 0.1)
          color.setX(intersects[0].face.c, 0.1)
          color.setY(intersects[0].face.a, 0.5)
          color.setY(intersects[0].face.b, 0.5)
          color.setY(intersects[0].face.c, 0.5)
          color.setZ(intersects[0].face.a, 1)
          color.setZ(intersects[0].face.b, 1)
          color.setZ(intersects[0].face.c, 1)
          intersects[0].object.geometry.attributes.color.needsUpdate = true
          const initialColor = {
            r:0,
            g:0.19,
            b:0.4
          }
          const hoverColor = {
            r:0.3,
            g:0.8,
            b:1
          }
          gsap.to(hoverColor, {
            r:initialColor.r,
            g:initialColor.g,
            b:initialColor.b,
            duration: 1.5,
            onUpdate: () =>{
              color.setX(intersects[0].face.a, hoverColor.r)
              color.setX(intersects[0].face.b, hoverColor.r)
              color.setX(intersects[0].face.c, hoverColor.r)
              color.setY(intersects[0].face.a, hoverColor.g)
              color.setY(intersects[0].face.b, hoverColor.g)
              color.setY(intersects[0].face.c, hoverColor.g)
              color.setZ(intersects[0].face.a, hoverColor.b)
              color.setZ(intersects[0].face.b, hoverColor.b)
              color.setZ(intersects[0].face.c, hoverColor.b)
              color.needsUpdate = true
            }
          })
        }
        stars.rotation.x += 0.001
      }
      const mouse = {
        x: undefined,
        y: undefined
      }
      animate()

      addEventListener('mousemove', (event) => {
        mouse.x = (event.clientX/innerWidth) * 2 -1
        mouse.y = -(event.clientY/innerHeight) * 2 +1
      })

      gsap.to('#albert', {
        opacity: 1,
        duration: 1.5,
        y: 0,
        ease: 'expo'
      })
      gsap.to('#pg1', {
        opacity: 1,
        duration: 1.5,
        delay:0.5,
        y:0,
        ease: 'expo'
      })
      gsap.to('#bbb', {
        opacity: 1,
        duration: 1.5,
        delay: 1,
        y: 0,
        ease: 'expo'
      })

      document.querySelector('#bbb').
      addEventListener('click', (e) => {
        e.preventDefault()
        console.log("go!")
        gsap.to('#div', {
          opacity: 0
        })
        gsap.to(camera.position, {
          z: 28,
          ease: 'expo.inOut',
          duration: 2.1
        })
        gsap.to(camera.rotation, {
          x: 1.57,
          ease: 'expo.inOut',
          duration: 1.5
        })
        gsap.to(camera.position, {
          y: 1000,
          ease: 'power3.in',
          duration: 2.4,
          delay: 1.5,
          onComplete: () => {
            this.$router.push('/work')
          }
        })
      })

      addEventListener('resize', () =>{
        camera.aspect = innerWidth / innerHeight
        camera.updateProjectionMatrix()
        renderer.setSize(innerWidth, innerHeight)
      })






  }
}
</script>

<style>
</style>
