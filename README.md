<h1 align="center">
  <a href="https://github.com/yandeu/enable3d#readme"><img src="readme/enable3d-logo-square.png" alt="enable3d logo" width="300"></a>
  <br>
  3D extension for Phaser 3
  <br>
</h1>

<h4 align="center">
Written in TypeScript, uses three.js and ammo.js, brings the third dimension to your Phaser 3 game.</h4>

<p align="center">  
  <a href="https://www.npmjs.com/package/enable3d"><img src="https://img.shields.io/npm/v/enable3d?style=flat-square" alt="NPM version"></a>
  <a href="https://github.com/yandeu/enable3d/commits/master"><img src="https://img.shields.io/github/last-commit/yandeu/enable3d.svg?style=flat-square" alt="GitHub last commit"></a>
  <a href="https://github.com/prettier/prettier" alt="code style: prettier"><img src="https://img.shields.io/badge/code_style-prettier-ff69b4.svg?style=flat-square"></a>
  <a href="https://www.typescriptlang.org/"><img src="https://img.shields.io/badge/built%20with-TypeScript-blue?style=flat-square"></a>
</p>

<p align="center">
  <a href="#key-features">Key Features</a> •
  <a href="#examples">Examples</a> •
  <a href="#get-started">Get Started</a> •
  <a href="#how-to-use-with-phaser">Use with Phaser</a> •
  <a href="#standalone-mode">Standalone Mode</a> •
  <a href="#phaser-starter-template">Phaser Starter Template</a> •
  <a href="#documentation">Documentation</a> •
  <a href="#license">License</a>
</p>

## Key Features

- Convert coordinates between Phaser 2D and three.js 3D elements
- Constraints
- Collision and Overlap detection
- and more...

## Examples

Find some nice examples on [enable3d.io](https://enable3d.io/examples.html)

## Get Started

Install phaser and enable3d via npm:

```console
npm i phaser enable3d
```

Or download the UMD bundle in [/bundles](https://github.com/yandeu/enable3d/tree/master/bundles)
The bundle exports "ENABLE3D".

```ts
const { enable3d, Scene3d } = ENABLE3D
// now use enable3d and Scene3d in your project
```

## How to use with Phaser

```ts
// STEP 1: Add the libraries with "npm install phaser enable3d"
import Phaser from 'phaser'
import enable3d, { Scene3D, Canvas } from 'enable3d'

const config = {
  // STEP 2: Set type to Phaser.WEBGL
  type: Phaser.WEBGL,
  backgroundColor: '#ffffff',
  scale: {
    parent: 'phaser-game',
    mode: Phaser.Scale.FIT,
    autoCenter: Phaser.Scale.CENTER_BOTH,
    width: 1280,
    height: 720
  },
  scene: [MainScene],
  // STEP 3: Add a custom canvas
  // The default Phaser canvas is not compatible with three.js
  ...Canvas()
}

window.addEventListener('load', () => {
  // STEP 4: Wrap enable3d around your Phaser game.
  // (First copy all ammo file from 'node_modules/enable3d/lib/ammo' to your public folder.)
  enable3d(() => new Phaser.Game(config)).withPhysics('/js/ammo')
})

// STEP 5: Extend your Scene with Scene3D instead of Phaser.Scene
class MainScene extends Scene3D {
  constructor() {
    super({ key: 'MainScene' })
  }

  init() {
    // STEP 6: Request a worm whole to the third dimension.
    this.requestThirdDimension()
  }

  create() {
    // STEP 7: Drive through the hole into the third dimension.
    this.accessThirdDimension()

    // STEP 8: Journey through the third dimension at warp speed.
    this.third.warpSpeed()

    // STEP 9: Add your first 3d object.
    this.third.add.box()

    // STEP 10: Add another box with physics enabled.
    this.third.physics.add.box()

    // STEP 11: Have fun using the third dimension in your awesome Phaser game.
    this.third.haveSomeFun()
  }
}
```

## Standalone Mode

At the moment enable3d can only be used together with Phaser.

## Phaser Starter Template

Looking for a powerful Phaser Starter Template?

Try [phaser-project-template](https://github.com/yandeu/phaser-project-template) or [phaser-project-template-es6](https://github.com/yandeu/phaser-project-template-es6)

## Documentation

### Object Factory

#### Adding objects

```ts
// Make the sphere object.
const sphere = this.third.make.sphere()
// Add the sphere to the scene.
this.third.add.existing(sphere)
// Add physics to the sphere.
this.third.physics.add.existing(sphere)

// The three lines above do the same thing as this:
this.third.physics.add.sphere()
```

#### Object types

For now you can use the following objects: Sphere, Box, Ground (which is a extended box).

```ts
// Adds a simple box.
this.third.add.box()
// Adds a simple sphere.
this.third.add.sphere()
// Width and height are required for the ground object.
this.third.add.ground({ width: 10, height: 10 })
```

#### Object Configuration and Material

Each geometry receives 2 objects. The first for the configuration of the shape, the second for its material.

```ts
this.third.add.box({ x: 10, y: 10, z: -25, width: 2 }, { standard: { color: 0xff00ff, metalness: 0.7 } })

this.third.add.sphere({ radius: 2 }, { phong: { color: 0xff00ff, wireframe: true } })
```

#### Object with textures

```ts
const grass = this.third.getTexture('grass')
this.third.physics.add.ground({ width: 50, height: 50, y: -5 }, { phong: { map: grass } })
```

### Collisions

```ts
const ground = this.third.physics.add.ground({ name: 'ground', width: 10, height: 10 })
const redBall = this.third.physics.add.sphere({ y: 10 }, { standard: { color: 0xff0000 } })
const blueBall = this.third.physics.add.sphere({ y: 15 }, { standard: { color: 0x0000ff } })
const greenBall = this.third.physics.add.sphere({ y: 20 }, { standard: { color: 0x00ff00 } })

// Check collision between the red and the blue ball.
this.third.physics.add.collider(redBall, blueBall, event => {
  // event will be 'start', 'collision' or 'end'
  if (event === 'start') {
    console.log('redBall and blueBall start colliding')
  }
})

// Detect all collisions of the green ball.
greenBall.body.on.collision((otherObject, event) => {
  if (otherObject.name === 'ground') console.log('The green ball collides with the ground')
  else console.log('The green ball collides with another ball')
})
```

## License

The GNU General Public License v3.0 (GNU GPLv3) 2019 - [Yannick Deubel](https://github.com/yandeu). Please have a look at the [LICENSE](LICENSE) for more details.
