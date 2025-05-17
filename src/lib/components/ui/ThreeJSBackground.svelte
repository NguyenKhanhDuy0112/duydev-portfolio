<!-- src/lib/components/ui/ThreeJSBackground.svelte -->
<script>
  import { onMount, onDestroy } from 'svelte';
  import * as THREE from 'three';
  
  let container;
  let scene, camera, renderer;
  let particles;
  let animationId;
  let mouseX = 0, mouseY = 0;
  let windowHalfX = window.innerWidth / 2;
  let windowHalfY = window.innerHeight / 2;
  
  function init() {
    // Scene setup
    scene = new THREE.Scene();
    
    // Camera setup
    camera = new THREE.PerspectiveCamera(
      75,
      window.innerWidth / window.innerHeight,
      0.1,
      2000
    );
    camera.position.z = 1000;
    
    // Particles
    const particlesGeometry = new THREE.BufferGeometry();
    const particlesCount = 15000;
    
    const posArray = new Float32Array(particlesCount * 3);
    const colorArray = new Float32Array(particlesCount * 3);
    
    for (let i = 0; i < particlesCount * 3; i++) {
      // Position
      posArray[i] = (Math.random() - 0.5) * 2000;
      
      // Color - create a blue to purple gradient
      const color = new THREE.Color();
      
      // Mix of blue and purple
      const hue = 0.6 + Math.random() * 0.1; // Blue to purple range (0.6-0.7)
      const saturation = 0.5 + Math.random() * 0.3; // Medium to high saturation
      const lightness = 0.4 + Math.random() * 0.3; // Medium lightness
      
      color.setHSL(hue, saturation, lightness);
      
      colorArray[i * 3] = color.r;
      colorArray[i * 3 + 1] = color.g;
      colorArray[i * 3 + 2] = color.b;
    }
    
    particlesGeometry.setAttribute('position', new THREE.BufferAttribute(posArray, 3));
    particlesGeometry.setAttribute('color', new THREE.BufferAttribute(colorArray, 3));
    
    // Material
    const particlesMaterial = new THREE.PointsMaterial({
      size: 3,
      vertexColors: true, 
      transparent: true,
      opacity: 0.8,
      sizeAttenuation: true,
      blending: THREE.AdditiveBlending
    });
    
    // Mesh
    particles = new THREE.Points(particlesGeometry, particlesMaterial);
    scene.add(particles);
    
    // Renderer
    renderer = new THREE.WebGLRenderer({ alpha: true, antialias: true });
    renderer.setPixelRatio(window.devicePixelRatio);
    renderer.setSize(window.innerWidth, window.innerHeight);
    container.appendChild(renderer.domElement);
    
    // Event Listeners
    document.addEventListener('mousemove', onDocumentMouseMove);
    window.addEventListener('resize', onWindowResize);
  }
  
  function onDocumentMouseMove(event) {
    mouseX = (event.clientX - windowHalfX) * 0.05;
    mouseY = (event.clientY - windowHalfY) * 0.05;
  }
  
  function onWindowResize() {
    windowHalfX = window.innerWidth / 2;
    windowHalfY = window.innerHeight / 2;
    
    camera.aspect = window.innerWidth / window.innerHeight;
    camera.updateProjectionMatrix();
    
    renderer.setSize(window.innerWidth, window.innerHeight);
  }
  
  function animate() {
    animationId = requestAnimationFrame(animate);
    
    // Rotate particles and respond to mouse movement
    if (particles) {
      particles.rotation.x += 0.0001;
      particles.rotation.y += 0.0001;
      
      // Move towards mouse position with lag
      particles.rotation.x += (mouseY * 0.0001 - particles.rotation.x) * 0.05;
      particles.rotation.y += (mouseX * 0.0001 - particles.rotation.y) * 0.05;
    }
    
    render();
  }
  
  function render() {
    renderer.render(scene, camera);
  }
  
  onMount(() => {
    if (typeof window !== 'undefined') {
      init();
      animate();
    }
  });
  
  onDestroy(() => {
    if (animationId) {
      cancelAnimationFrame(animationId);
    }
    
    if (renderer) {
      renderer.dispose();
    }
    
    document.removeEventListener('mousemove', onDocumentMouseMove);
    window.removeEventListener('resize', onWindowResize);
  });
</script>

<div bind:this={container} class="absolute inset-0 -z-10"></div>

<style>
  div {
    background: linear-gradient(to bottom, #0f172a, #1e293b);
  }
</style>