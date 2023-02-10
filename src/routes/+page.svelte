<script>
	import { Canvas, InteractiveObject, OrbitControls, T } from '@threlte/core'
	import { spring } from 'svelte/motion'
	import { degToRad } from 'three/src/math/MathUtils'
  import { onMount } from 'svelte'

	import { GLTF} from '@threlte/extras'

  let y;

	let scale = spring(1)

  function scroll() {
    scale = spring(y/50);
    camera.position = [y/50 + 10, y/50+10, y/50+ 10]
    console.log(y/10);
    requestAnimationFrame(scroll)
  }

  let camera = {
    position: [10,10,10],
    fov: 24
  }

  onMount(() => {
    scroll()
  })

</script>

<svelte:window bind:scrollY={y} />

<div class="full">
	<Canvas>
		<T.PerspectiveCamera makeDefault {...camera}>
			<OrbitControls maxPolarAngle={degToRad(80)} enableZoom={false} target={{ y: 0.5 }} />
		</T.PerspectiveCamera>

		<T.DirectionalLight castShadow position={[3, 10, 10]} />
		<T.DirectionalLight position={[-3, 10, -10]} intensity={0.2} />
		<T.AmbientLight intensity={0.2} />

		<!-- Cube -->
			<GLTF scale={$scale} url="https://threejs.org/examples/models/gltf/Xbot.glb"/>
		<T.Group scale={$scale}>
		</T.Group>

		<!-- Floor -->
		<T.Mesh receiveShadow rotation.x={degToRad(-90)}>
			<T.CircleGeometry args={[3, 72]} />
			<T.MeshStandardMaterial color="white" />
		</T.Mesh>
	</Canvas>
</div>
<section>
  test
</section>
<section>
  test2
</section>

<style>
	 .full {
     position: fixed;
		height: 100%;
		width: 100%;
	}

  section {
    min-height: 100%;
  }
</style>