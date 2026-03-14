<template>
    <div class="container mx-auto p-4">
        <div class="coffee-content">
            <h2>3D coffee machine</h2>
            <p class="mb-6">The 3D coffee machine model presents a detailed and interactive visualization of a modern coffee brewing device. Designed to demonstrate both form and function, the model highlights key components such as the water reservoir, heating system, brewing chamber, and coffee outlet. Through this three-dimensional representation, users can better understand how coffee beans are transformed into a freshly brewed cup of coffee. The model allows viewers to explore the machine from different angles, making the brewing process more engaging and easier to explain. By combining realistic structure with clear visual design, this 3D coffee machine serves as an effective educational tool for illustrating the coffee-making process in an interactive infographic or digital learning environment.</p>
            <div ref="coffee_3d" class="overflow-hidden border border-gray-400" style="width: 100%; height: 100vh;"></div>
        </div>
    </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import * as THREE from 'three';
import { GLTFLoader } from 'three/addons/loaders/GLTFLoader.js';
import { OrbitControls } from 'three/addons/controls/OrbitControls.js';
import coffeeModel from '@/assets/coffee_machine.glb';

const coffee_3d = ref(null);

onMounted(() => {
  // Scene setup
  const scene = new THREE.Scene();
  scene.background = new THREE.Color(0xffffff);

  // Get container dimensions
  const width = coffee_3d.value.clientWidth;
  const height = coffee_3d.value.clientHeight;

  // Camera setup
  const camera = new THREE.PerspectiveCamera(
    75,
    width / height,
    0.1,
    1000
  );
  camera.position.set(0, 0, 5);

  // Renderer setup
  const renderer = new THREE.WebGLRenderer({ antialias: true });
  renderer.setSize(width, height);
  renderer.setPixelRatio(window.devicePixelRatio);
  coffee_3d.value.appendChild(renderer.domElement);

  // OrbitControls for drag and rotate
  const controls = new OrbitControls(camera, renderer.domElement);
  controls.enableDamping = true;
  controls.dampingFactor = 0.05;
  controls.autoRotate = true;
  controls.autoRotateSpeed = 5;
  controls.target.set(0, 0, 0);

  // Lighting
  const light = new THREE.DirectionalLight(0xffffff, 1);
  light.position.set(5, 5, 5);
  scene.add(light);

  const ambientLight = new THREE.AmbientLight(0xffffff, 0.5);
  scene.add(ambientLight);

  // Load glTF model
  const loader = new GLTFLoader();
  loader.load(
    coffeeModel,
    (gltf) => {
      gltf.scene.scale.set(2, 2, 2);
      // compute bounding box center
      const box = new THREE.Box3().setFromObject(gltf.scene);
      const center = box.getCenter(new THREE.Vector3());
      // subtract center to move model origin to scene origin
      gltf.scene.position.sub(center);

      scene.add(gltf.scene);

      // ensure camera is looking at model center (now origin)
      camera.lookAt(0, 0, 0);
    },
    (progress) => {
      //console.log('Loading...', (progress.loaded / progress.total * 100) + '%');
    },
    (error) => {
      //console.error('Error loading model:', error);
    }
  );

  // Animation loop
  const animate = () => {
    requestAnimationFrame(animate);
    controls.update();
    renderer.render(scene, camera);
  };
  animate();

  // Handle window resize
  const handleResize = () => {
    if (!coffee_3d.value) return;
    const width = coffee_3d.value.clientWidth;
    const height = coffee_3d.value.clientHeight;
    camera.aspect = width / height;
    camera.updateProjectionMatrix();
    renderer.setSize(width, height);
  };
  window.addEventListener('resize', handleResize);
});
</script>

<style scoped>
.coffee-content {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
}

h2 {
  color: #2c3e50;
  font-size: 2.5rem;
  margin-bottom: 20px;
  text-align: center;
}
</style>