<script lang="ts">

</script>

<script setup lang="ts">
import Phaser from 'phaser'
import { ref, onMounted, type Ref } from 'vue'

let player: any
let dustTrails : Phaser.GameObjects.Particles.ParticleEmitter
let SPEED = 3
const velocity: Ref<number> = ref(150)
const config: Phaser.Types.Core.GameConfig = {
  type: Phaser.AUTO,
  width: 800,
  height: 600,
  backgroundColor: 'gray',
  scene: {
    preload: preload,
    create: create,
    update: update
  }
}


function preload() {
    //@ts-expect-error: becouse of this
    const scene: Phaser.Scene = this
  scene.load.setBaseURL('http://labs.phaser.io');

  scene.load.image('red', 'assets/particles/red.png');
}

function create() {
  // while creating
  //@ts-expect-error: becouse of this
  const scene: Phaser.Scene = this

  const trail = scene.add.particles('red')

  //  scene.tweens.add({

  //     targets: r1,
  //     x : 800,
  //     y: 600,
  //     yoyo: true,
  //     repeat: -1,
  //     ease: 'Sine.ease',
  //     duration : 5000

  // })
  console.log(scene)


  player = scene.add.circle(100, 100, 7,  hexConverter('#ffffff'), 1)

  dustTrails = trail.createEmitter({
            speed: 100,
            scale: { start: 0.3, end: 0 },
            blendMode: 'ADD',
            lifespan : 250,
            x : player.x,
            y : player.y,
            alpha : 0.3,
            gravityX : 15,
            gravityY : 25 
            
  });

}

function update() {
  //@ts-expect-error: becouse of this
  const scene: Phaser.Scene = this
  const cursors = scene.input.keyboard.createCursorKeys()
  playerController(cursors,scene)

}


function playerController(cursors : Phaser.Types.Input.Keyboard.CursorKeys, scene : Phaser.Scene){
 
  const dustMaker = () => {
    dustTrails.explode(1, player.x, player.y)
    dustTrails.startFollow(player)
  }

  if (cursors.up.isDown) {
    player.y -= SPEED 
    dustMaker()
    
  }
  if (cursors.down.isDown) {
    player.y += SPEED
    dustMaker()
   
  }
  if (cursors.left.isDown) {
    player.x -= SPEED
    dustMaker()
    
  }
  if (cursors.right.isDown) {
    player.x += SPEED
    dustMaker()
    
    
  }


  
  if(cursors.space.isDown){
    SPEED = 5
    // hook like state
    // scene.add.tween({
    //   targets :player,
    //   x : player.x  * 2,
    //   duration: 500
    // })

  }

}

onMounted(() => {
  const game: Phaser.Game = new Phaser.Game(config)
  console.log(game)

})


function hexConverter(color : string) : any {
  return color.replace('#','0x')
}


function RNG(maximum: number, minimum: number): number {
  return Math.floor(Math.random() * (maximum - minimum + 1)) + minimum
}
</script>

<template>
  <main>

  </main>
</template>

<style scoped>

</style>
