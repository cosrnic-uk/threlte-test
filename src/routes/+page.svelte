<script>
	import { Canvas, InteractiveObject, OrbitControls, T } from '@threlte/core'
	import { spring } from 'svelte/motion'
	import { degToRad } from 'three/src/math/MathUtils'
  import { onMount } from 'svelte'

  let y;

	let scale = spring(1)

  function scroll() {
    scale = spring(1);
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
		<T.Group scale={$scale}>
			<T.Mesh position.y={0.5} castShadow let:ref>
				<!-- Add interaction -->
				<InteractiveObject
					object={ref}
					interactive
					on:pointerenter={() => ($scale = 2)}
					on:pointerleave={() => ($scale = 1)}
				/>

				<T.BoxGeometry />
				<T.MeshStandardMaterial color="#333333" />
			</T.Mesh>
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