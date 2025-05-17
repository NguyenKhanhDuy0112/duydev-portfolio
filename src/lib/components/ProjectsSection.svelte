<script>
  import { onMount } from 'svelte';
  import { fly } from 'svelte/transition';
  import { quintOut } from 'svelte/easing';
  import { projects } from '$lib/data/projects';
  import ProjectCard from '$lib/components/ui/ProjectCard.svelte';
  import { intersectionObserver } from '$lib/utils/intersectionObserver';
  
  let visible = false;
  let currentIndex = 0;
  let swiperContainer;
  let swiping = false;
  
  function nextProject() {
    if (swiping) return;
    swiping = true;
    
    currentIndex = (currentIndex + 1) % projects.length;
    
    setTimeout(() => {
      swiping = false;
    }, 600);
  }
  
  function prevProject() {
    if (swiping) return;
    swiping = true;
    
    currentIndex = (currentIndex - 1 + projects.length) % projects.length;
    
    setTimeout(() => {
      swiping = false;
    }, 600);
  }
  
  onMount(() => {
    const section = document.getElementById('projects');
    
    if (section) {
      intersectionObserver(section, () => {
        visible = true;
      });
    }
    
    // Initialize touch swipe detection
    if (swiperContainer) {
      let touchStartX = 0;
      
      swiperContainer.addEventListener('touchstart', (e) => {
        touchStartX = e.changedTouches[0].screenX;
      }, { passive: true });
      
      swiperContainer.addEventListener('touchend', (e) => {
        const touchEndX = e.changedTouches[0].screenX;
        const diff = touchStartX - touchEndX;
        
        if (Math.abs(diff) > 50) { // Minimum swipe distance
          if (diff > 0) {
            nextProject();
          } else {
            prevProject();
          }
        }
      }, { passive: true });
    }
  });
</script>

<section id="projects" class="py-24 relative">
  <!-- Background Gradient & Pattern -->
  <div class="absolute inset-0 bg-gradient-to-b from-gray-800 to-gray-900 opacity-90 -z-10"></div>
  <div class="absolute inset-0 bg-[url('/dots-pattern.svg')] bg-repeat opacity-10 -z-10"></div>
  
  <div class="container mx-auto px-6">
    <!-- Section Title -->
    {#if visible}
      <div 
        class="text-center mb-16" 
        in:fly={{ y: -20, duration: 800, delay: 100 }}
      >
        <h2 class="text-4xl md:text-5xl font-bold text-white mb-4">
          Featured Projects
        </h2>
        <div class="w-24 h-1 bg-gradient-to-r from-blue-500 to-purple-500 mx-auto"></div>
      </div>
    
      <!-- Project Carousel -->
      <div 
        bind:this={swiperContainer}
        class="relative mx-auto max-w-5xl overflow-hidden" 
        in:fly={{ y: 40, duration: 800, delay: 200 }}
      >
        <div class="flex transition-transform duration-500 ease-out" style="transform: translateX(-{currentIndex * 100}%)">
          {#each projects as project, i}
            <div class="min-w-full px-4">
              <ProjectCard {project} active={i === currentIndex} />
            </div>
          {/each}
        </div>
        
        <!-- Navigation Arrows -->
        <button 
          class="absolute left-4 top-1/2 transform -translate-y-1/2 w-12 h-12 rounded-full bg-gray-800/80 text-white flex items-center justify-center z-10 hover:bg-gray-700 transition-colors duration-300"
          on:click={prevProject}
          aria-label="Previous project"
        >
          <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 19l-7-7 7-7" />
          </svg>
        </button>
        
        <button 
          class="absolute right-4 top-1/2 transform -translate-y-1/2 w-12 h-12 rounded-full bg-gray-800/80 text-white flex items-center justify-center z-10 hover:bg-gray-700 transition-colors duration-300"
          on:click={nextProject}
          aria-label="Next project"
        >
          <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7" />
          </svg>
        </button>
        
        <!-- Pagination Dots -->
        <div class="flex justify-center space-x-2 mt-8">
          {#each projects as _, i}
            <button 
              class="w-3 h-3 rounded-full transition-all duration-300 {i === currentIndex ? 'bg-gradient-to-r from-blue-500 to-purple-500 scale-125' : 'bg-gray-600 hover:bg-gray-500'}"
              on:click={() => currentIndex = i}
              aria-label={`Go to project ${i + 1}`}
            ></button>
          {/each}
        </div>
      </div>
    {/if}
  </div>
</section>