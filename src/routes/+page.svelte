<script>
    import { Canvas, InteractiveObject, OrbitControls, T } from "@threlte/core";
    import { spring } from "svelte/motion";
    import { degToRad } from "three/src/math/MathUtils";
    import { GLTF } from "@threlte/extras";
    import { AmbientLight } from "@threlte/core";
    import { fade } from "svelte/transition";
    const scale = spring(1);
    let deg = 80;
    let x;
</script>

<div class="min-h-[3000px] px-10 text-white">
    <div class="fixed left-0 bg-yellow-500  ">
        {#if x < 100}
            <h1
                class="absolute bottom-0 mx-auto font-extrabold text-[170px] translate-x-10 z-50"
                transition:fade
            >
                Scroll Down.
            </h1>
        {/if}
        {#if x > 200 && x < 400}
            <h1
                class="absolute font-extrabold text-[170px] translate-x-10 z-50"
                transition:fade
            >
                Hello.
            </h1>
        {/if}
        {#if x > 400}
            <h1
                class=" absolute bottom-10 font-extrabold text-[150px] translate-x-10 z-50"
                transition:fade
            >
                Im Cepi.
            </h1>
        {/if}
        {#if x > 600}{/if}
    </div>

    <div class="fixed left-0 mx-auto top-0 bottom-0 w-full ">
        {#if x > 700}
            <h1
                class="absolute bottom-0  text-3xl font-extrabold"
                transition:fade
                style="transform: translate(960px, -500px);"
            >
                Frontend Developer
            </h1>
            <p
                class="absolute bottom-0  text-lg font-extrabold"
                transition:fade
                style="transform: translate(960px, -480px);"
            >
                at cnt.id
            </p>
            <button
                transition:fade
                class="absolute bottom-0   border border-white p-3 z-50"
                style="transform: translate(960px, -420px);"
            >
                <a href="https://www.linkedin.com/in/cepi-herdiansyah/">
                    Hire me !</a
                >
            </button>
        {/if}
        <Canvas>
            <T.PerspectiveCamera makeDefault position={[40, 90, 50]} fov={20}>
                <OrbitControls
                    maxPolarAngle={degToRad(40)}
                    enableZoom={false}
                    target={{ y: 0, z: 1 }}
                />
            </T.PerspectiveCamera>

            <T.DirectionalLight castShadow position={[3, 2, 10]} />
            <T.DirectionalLight position={[-3, 40, -10]} intensity={0.2} />
            <AmbientLight
                color={0x3a86ff}
                intensity={0.2}
                position={{ y: 2 }}
            />
            <!-- <AmbientLight color={0x8338ec} intensity={0.1} /> -->
            <!-- <AmbientLight color={0xff006e} intensity={0.1} /> -->

            <!-- Cube -->
            <T.Group scale={$scale}>
                <T.Mesh position.y={1} castShadow let:ref>
                    <!-- Add interaction -->
                    <InteractiveObject
                        object={ref}
                        interactive
                        on:pointerenter={() => ($scale = 2)}
                        on:pointerleave={() => ($scale = 1)}
                        on:click={() => console.log("sad")}
                    />
                    <GLTF
                        castShadow
                        receiveShadow
                        url={"./tree.glb"}
                        interactive
                        scale={0.4 + x / 1000}
                        position={{ y: 0.1 }}
                    />
                    <GLTF
                        castShadow
                        receiveShadow
                        url={"./cybertruck.glb"}
                        interactive
                        scale={0.4 + x / 1000}
                        position={{ x: 1 + x / 30 }}
                        rotation={x / 100}
                    />

                    <GLTF
                        castShadow
                        receiveShadow={true}
                        url={"./cybertruck.glb"}
                        interactive
                        scale={0.4 + x / 1000}
                        position={{ x: -70 + x / 30, z: 8 }}
                    />

                    <GLTF
                        castShadow
                        receiveShadow={true}
                        url={"./cnt.glb"}
                        interactive
                        scale={0.4 + x / 1000}
                        position={{ y: 0, z: -4 - x / 100 , x:-2 + x/10000 }}
                    />
                    <!-- <T.BoxGeometry  /> -->
                    <T.MeshStandardMaterial color="#ffffff" />
                </T.Mesh>
            </T.Group>

            <!-- Floor -->
            <T.Mesh receiveShadow rotation.x={degToRad(-90)}>
                <!-- <T.BoxGeometry args={[20, 10]} /> -->
                <T.MeshStandardMaterial color="#ffffff" />
            </T.Mesh>
        </Canvas>
    </div>
</div>

<svelte:window bind:scrollY={x} />

<style>
    div {
        height: 100%;
        width: 100%;
        /* background-color: black; */
        margin: 0;
    }
</style>
