<!-- src/lib/components/HeroSection.svelte -->
<script>
  import { onMount } from 'svelte';
  import { fade, fly } from 'svelte/transition';
  import { spring } from 'svelte/motion';
  import ThreeJSBackground from '$lib/components/ui/ThreeJSBackground.svelte';
  
  let visible = false;
  
  // Animated coordinates for the CTA button
  const coords = spring({ x: 0, y: 0 }, {
    stiffness: 0.1,
    damping: 0.25
  });
  
  // Handling mouse movement for the CTA button glow effect
  function handleMouseMove(event) {
    const { clientX, clientY, currentTarget } = event;
    const rect = currentTarget.getBoundingClientRect();
    
    coords.set({
      x: clientX - rect.left - rect.width / 2,
      y: clientY - rect.top - rect.height / 2
    });
  }
  
  // Reset animation when mouse leaves
  function handleMouseLeave() {
    coords.set({ x: 0, y: 0 });
  }
  
  onMount(() => {
    // Delay to ensure Three.js has loaded
    setTimeout(() => {
      visible = true;
    }, 100);
  });
</script>

<section id="hero" class="relative min-h-screen flex items-center justify-center overflow-hidden">
  <!-- 3D Background -->
  <ThreeJSBackground />
  
  <!-- Content -->
  {#if visible}
    <div 
      class="container mx-auto px-6 z-10 text-center" 
      in:fade={{ delay: 300, duration: 1000 }}
    >
      <div in:fly={{ y: 40, duration: 1000, delay: 300 }}>
        <h1 class="text-5xl md:text-7xl font-bold mb-2 text-white">
          DUY NGUYEN
        </h1>
        
        <h2 class="text-2xl md:text-3xl mb-12 text-blue-400 font-light">
          <span class="typewriter">Fullstack Developer</span>
        </h2>
      </div>
      
      <div class="flex flex-col sm:flex-row gap-6 justify-center mt-12" in:fly={{ y: 60, duration: 1000, delay: 600 }}>
        <a 
          href="#projects"
          class="btn-primary relative overflow-hidden"
          on:mousemove={handleMouseMove}
          on:mouseleave={handleMouseLeave}
        >
          <span class="relative z-10">View My Work</span>
          
          <!-- Glow Effect -->
          <span
            class="absolute inset-0 w-full h-full bg-gradient-to-r from-blue-500 to-purple-500 opacity-70 blur-2xl transition-all duration-200 ease-in-out"
            style="transform: translate({$coords.x / 5}px, {$coords.y / 5}px)"
          ></span>
        </a>
        
        <a 
          href="#contact" 
          class="btn-secondary"
        >
          Get In Touch
        </a>
      </div>
      
      <div class="absolute bottom-10 left-1/2 transform -translate-x-1/2 animate-bounce">
        <a href="#about" aria-label="Scroll down">
          <svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8 text-white" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 14l-7 7m0 0l-7-7m7 7V3" />
          </svg>
        </a>
      </div>
    </div>
  {/if}
</section>

<style lang="postcss">
  .btn-primary {
    @apply bg-gradient-to-r from-blue-600 to-purple-600 
      py-3 px-8 rounded-md font-medium text-white 
      shadow-lg hover:shadow-blue-600/50 
      transition-all duration-300 transform hover:-translate-y-1;
  }
  
  .btn-secondary {
    @apply bg-gray-800 border border-gray-600 
      py-3 px-8 rounded-md font-medium text-white 
      shadow-lg hover:shadow-purple-600/20 
      transition-all duration-300 transform hover:-translate-y-1 hover:bg-gray-700;
  }
  
  .typewriter {
    @apply border-r-4 border-blue-400 pr-1;
    animation: typing 3.5s steps(40, end), blink-caret 0.75s step-end infinite;
    overflow: hidden;
    white-space: nowrap;
  }
  
  @keyframes typing {
    from { width: 0 }
    to { width: 100% }
  }
  
  @keyframes blink-caret {
    from, to { border-color: transparent }
    50% { border-color: theme('colors.blue.400') }
  }
</style>