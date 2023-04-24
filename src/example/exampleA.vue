<template>
    <div ref="threeRef"></div>
</template>

<script setup lang="ts">
import { onMounted, ref } from 'vue'
import { AxesHelper, BoxGeometry, Color, Mesh, MeshLambertMaterial, PerspectiveCamera, PlaneGeometry, Scene, SphereGeometry, SpotLight, WebGLRenderer } from 'three';

const threeRef = ref();

const init = () => {
    const scene = new Scene();
    const renderer = new WebGLRenderer();
    // 添加坐标系
    const axes = new AxesHelper(20);
    scene.add(axes);
    // 添加底板
    const planeGeometry = new PlaneGeometry(60, 20);
    const planeMaterial = new MeshLambertMaterial({ color: 0xcccccc });
    const plane = new Mesh(planeGeometry, planeMaterial);
    plane.receiveShadow = true;

    plane.rotation.x = -0.5 * Math.PI;

    plane.position.x = 0;
    plane.position.y = 0;
    plane.position.z = 0;

    scene.add(plane)
    // 添加正方体
    const cubeGeometry = new BoxGeometry(4, 4, 4)
    const cubeMaterial = new MeshLambertMaterial({ color: 0xff0000, wireframe: false })
    const cube = new Mesh(cubeGeometry, cubeMaterial);
    cube.castShadow = true;
    cube.position.x = -4;
    cube.position.y = 2;
    cube.position.z = 2;

    scene.add(cube)

    const sphereGeometry = new SphereGeometry(4, 20, 20);
    const sphereMaterial = new MeshLambertMaterial({ color: 0x7777ff });
    const sphere = new Mesh(sphereGeometry, sphereMaterial)
    sphere.castShadow = true;

    sphere.position.x = 10;
    sphere.position.y = 4
    sphere.position.y = 2;

    scene.add(sphere)

    const spotLight = new SpotLight(0xffffff);
    spotLight.position.set(-40, 60, -10);
    spotLight.castShadow = true;
    scene.add(spotLight)

    renderer.setClearColor(new Color(0xEEEEEE));
    renderer.setSize(window.innerWidth, window.innerHeight);
    renderer.shadowMap.enabled = true;

    const camera = new PerspectiveCamera(45, window.innerWidth / window.innerHeight, 0.1, 1000)
    camera.position.x = -30;
    camera.position.y = 40;
    camera.position.z = 30;
    camera.lookAt(scene.position)

    threeRef.value.appendChild(renderer.domElement);

    let step = 0;
    renderScene();
    function renderScene() {

        cube.rotation.x += 0.02;
        cube.rotation.y += 0.02;
        cube.rotation.z += 0.02;

        step += 0.04;
        sphere.position.x = 20 + 10 * Math.cos(step);
        sphere.position.y = 2 + 10 * Math.abs(Math.sin(step))

        requestAnimationFrame(renderScene)
        renderer.render(scene, camera);
    }

}
onMounted(() => {
    init();
})
</script>

<style lang="scss" scoped></style>