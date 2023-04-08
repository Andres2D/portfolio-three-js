## Project set up and helpers

### Project dependencies 
> npm i --legacy-peer-deps @react-three/fiber @react-three/drei maath react-tilt react-vertical-timeline-component @emailjs/browser framer-motion react-router-dom
> npm i --legacy-peers-deps -D postcss autoprefixer


## Creating a 3D model
### imports
```
import { Suspense, useEffect, useState } from 'react';
import { Canvas } from '@react-three/fiber';
import { OrbitControls, Preload, useGLTF } from '@react-three/drei';
```

### JSX Element
```jsx
// Element to render
const Computers = () => {
  // Import of the GLTF model scene
  const computer = useGLTF('./desktop_pc/scene.gltf');

  return (
    <mesh>
      <!-- The light -->
      <hemisphereLight 
        intensity={0.15}
        groundColor='black' 
      />
      <pointLight
        intensity={1}  
      />
      <spotLight 
        position={[-20, 50, 10]}
        angle={0.12}
        penumbra={1}
        intensity={1}
        castShadow
        shadow-mapSize={1024}
      />
      <primitive 
        object={computer.scene}
        scale={0.75}
        position={[0, -3.25, -1.5]}
        rotation={[-0.01, -0.2, -0.1]}
      />
    </mesh>
  )
}


// At the same file we need to add

const ComputerCanvas = () => {
  return (
    // The actual load of the scene
    <Canvas
      frameloop='demand'
      shadows
      camera={{ position: [20, 3, 5], fov: 25 }}
      gl={{ preserveDrawingBuffer: true }}
    >

    // Just to preload a kind of loader while the model is loading
      <Suspense fallback={<CanvasLoader />}>
        <OrbitControls 
          enableZoom={false}
          maxPolarAngle={Math.PI / 2}  --> avoid rotating on y
          minPolarAngle={Math.PI / 2}  --> avoid rotating on y
        />
        // The component created above
        <Computers />
      </Suspense>
      <Preload all />
    </Canvas>
  )
}

export default ComputerCanvas;
```

In order to load the possible heavy model, we need to implement a loader like this:

```jsx
import { Html, useProgress } from '@react-three/drei';

const Loader = () => {

  const { progress } = useProgress();

  return (
    <Html>
      <span className='canvas-load'></span>
      <p
        style={{
          fontSize: 14,
          color: '#F1F1F1',
          fontWeight: 800,
          marginTop: 40
        }}
      >{progress.toFixed(2)}%</p>
    </Html>
  )
}

export default Loader

```

## Info
Based on the tutorial: https://www.youtube.com/watch?v=0fYi8SGA20k&t=306s

Gist with base data: https://gist.github.com/adrianhajdin/b1d33c262941a7e21aad833a1cfc84b1

Assets: https://drive.google.com/drive/folders/1KVU8iaH0E_JFtShNiR3BgCSA3pawXY4Z

