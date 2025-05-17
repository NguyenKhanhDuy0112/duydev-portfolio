<script>
  import { onMount } from 'svelte';
  import { fly } from 'svelte/transition';
  import { quintOut } from 'svelte/easing';
  import { experience } from '$lib/data/experience';
  import TimelineItem from '$lib/components/ui/TimelineItem.svelte';
  import { intersectionObserver } from '$lib/utils/intersectionObserver';
  
  let visible = false;
  
  onMount(() => {
    const section = document.getElementById('experience');
    
    if (section) {
      intersectionObserver(section, () => {
        visible = true;
      });
    }
  });
</script>

<section id="experience" class="py-24 relative">
  <!-- Background Gradient -->
  <div class="absolute inset-0 bg-gradient-to-b from-gray-900 to-gray-800 -z-10"></div>
  
  <div class="container mx-auto px-6">
    <!-- Section Title -->
    {#if visible}
      <div 
        class="text-center mb-16" 
        in:fly={{ y: -20, duration: 800, delay: 100 }}
      >
        <h2 class="text-4xl md:text-5xl font-bold text-white mb-4">
          Experience & Education
        </h2>
        <div class="w-24 h-1 bg-gradient-to-r from-blue-500 to-purple-500 mx-auto"></div>
      </div>
    
      <!-- Timeline -->
      <div class="relative max-w-4xl mx-auto">
        <!-- Vertical Timeline Line -->
        <div class="hidden md:block absolute left-1/2 transform -translate-x-1/2 h-full w-1 bg-gradient-to-b from-blue-600 to-purple-600 timeline-line"></div>
        
        <!-- Timeline Items -->
        {#each experience as item, i}
          <div in:fly={{ 
            x: i % 2 === 0 ? -30 : 30, 
            y: 20,
            duration: 800, 
            delay: 200 + (i * 100), 
            easing: quintOut 
          }}>
            <TimelineItem 
              {item} 
              index={i} 
              isLast={i === experience.length - 1} 
            />
          </div>
        {/each}
      </div>
    {/if}
  </div>
</section>

<style>
  .timeline-line {
    transform-origin: top;
    transform: scaleY(0);
  }
  
  /* Animation for the timeline line to draw as user scrolls */
  :global(.visible) .timeline-line {
    animation: draw-line 1.5s ease forwards;
  }
  
  @keyframes draw-line {
    from { transform: scaleY(0); }
    to { transform: scaleY(1); }
  }
</style>