<template>
  <object3d :position="pos" base-url="static/threex/spaceships/">
    <mesh>
      <m-obj-mtl obj-url="SpaceFighter03.obj"
          mtl-url="SpaceFighter03.mtl"
          :process="getBody">
      </m-obj-mtl>
    </mesh>

    <mesh v-for="n in 2" :key="`1-${n}`" :scale="4"
        :position="{ x: 5 * Math.sign(n - 1.5), z: 0.8 }">
      <geometry type="Plane" :args="[1, 1]"></geometry>
      <material type="MeshBasic" :options="detonation.matOpts">
        <texture url="lensflare0_alpha.png"></texture>
      </material>
    </mesh>

    <object3d v-for="n in 2" :key="`2-${n}`"
        :rotation="{ y: Math.PI / 2 }" :scale="4"
        :position="{ x: 5 * Math.sign(n - 1.5), z: 2.6 }">
      <mesh v-for="n1 in 4" :key="n1"
          :rotation="{ x: (n1 - 1) * Math.PI / 4 }">
        <geometry type="Plane" :args="[1, 1]"></geometry>
        <material type="MeshBasic" :options="shoot.matOpts">
          <texture :canvas="shoot.txtCanvas"></texture>
        </material>
      </mesh>
    </object3d>

    <animation :fn="animate"></animation>
  </object3d>
</template>

<script>
import * as THREE from 'three'
import { Object3D } from '@'

export default {
  name: 'SF03',
  mixins: [Object3D],

  data () {
    return {
      detonation: {
        matOpts: {
          color: 0x00ffff,
          side: THREE.DoubleSide,
          blending: THREE.AdditiveBlending,
          opacity: 2,
          depthWrite: false,
          transparent: true
        }
      },
      shoot: {
        matOpts: {
          color: 0xffaacc,
          side: THREE.DoubleSide,
          blending: THREE.AdditiveBlending,
          depthWrite: false,
          transparent: true
        },
        txtCanvas: this.generateShootCanvas()
      },
      pos: null
    }
  },

  methods: {
    animate (tt) {
      this.pos = { y: Math.sin(tt) }
    },

    getBody (group) {
      let body = group.children[0]
      body.material.color.set(0xffffff)
      return body
    },

    /* eslint-disable semi, space-in-parens */
    generateShootCanvas () {
      // init canvas
      var canvas = document.createElement( 'canvas' );
      var context = canvas.getContext( '2d' );
      canvas.width = 16;
      canvas.height = 64;
      // set gradient
      var gradient = context.createRadialGradient(
        canvas.width / 2, canvas.height / 2, 0,
        canvas.width / 2, canvas.height / 2, canvas.width / 2
      );
      gradient.addColorStop( 0, 'rgba(255,255,255,1)' );
      gradient.addColorStop( 0.5, 'rgba(192,192,192,1)' );
      gradient.addColorStop( 0.8, 'rgba(128,128,128,0.7)' );
      gradient.addColorStop( 1, 'rgba(0,0,0,0)' );

      // fill the rectangle
      context.fillStyle = gradient;
      context.fillRect(0, 0, canvas.width, canvas.height);
      // return the just built canvas
      return canvas;
    }
  }
}
</script>
