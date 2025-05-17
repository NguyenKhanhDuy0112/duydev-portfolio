<script>
  import { spring } from 'svelte/motion';
  
  export let item;
  export let index;
  export let isLast = false;
  
  // Animation for hover effect
  const scale = spring(1, {
    stiffness: 0.2,
    damping: 0.8
  });
  
  function handleMouseEnter() {
    scale.set(1.03);
  }
  
  function handleMouseLeave() {
    scale.set(1);
  }
</script>

<div class="relative flex flex-col md:flex-row items-center mb-8">
  <!-- Date bubble for mobile -->
  <div class="md:hidden absolute left-0 top-0 w-20 h-8 -ml-6 bg-gradient-to-r from-blue-600 to-purple-600 rounded-r-full flex items-center justify-center text-white text-xs font-medium z-10">
    {item.year}
  </div>

  <!-- Timeline content layout -->
  <div class="flex items-center w-full">
    <!-- Left side (if even index) -->
    <div class="hidden md:block md:w-1/2 {index % 2 === 0 ? 'pr-12 text-right' : 'order-2 pl-12'}">
      {#if index % 2 === 0}
        <div 
          style="transform: scale({$scale})"
          on:mouseenter={handleMouseEnter}
          on:mouseleave={handleMouseLeave}
          class="bg-gray-800 p-6 rounded-lg shadow-md transition-all duration-300 hover:shadow-xl border-r-4 border-blue-600"
        >
          <h3 class="text-xl font-bold text-white">{item.title}</h3>
          <p class="text-gray-300 font-medium mb-2">{item.subtitle}</p>
          <p class="text-blue-400 text-sm mb-2">{item.year}</p>
          <p class="text-gray-400">{item.description}</p>
        </div>
      {/if}
    </div>
    
    <!-- Timeline center dot -->
    <div class="flex flex-col items-center transform translate-y-4 md:transform-none">
      <div class="w-10 h-10 rounded-full bg-gradient-to-r from-blue-600 to-purple-600 shadow-lg shadow-blue-600/30 flex items-center justify-center z-10">
        <svg class="w-5 h-5 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 10V3L4 14h7v7l9-11h-7z"></path>
        </svg>
      </div>
      {#if !isLast}
        <div class="h-full w-1 bg-gradient-to-b from-blue-600 to-purple-600 md:hidden"></div>
      {/if}
    </div>
    
    <!-- Right side (if odd index) + Mobile content -->
    <div class="{index % 2 !== 0 ? 'hidden md:block md:w-1/2 pr-12 text-right' : 'order-2 md:w-1/2 pl-12'} md:pl-12">
      {#if index % 2 !== 0 || true}
        <div 
          style="transform: scale({$scale})"
          on:mouseenter={handleMouseEnter}
          on:mouseleave={handleMouseLeave}
          class="bg-gray-800 p-6 rounded-lg shadow-md transition-all duration-300 hover:shadow-xl {index % 2 !== 0 ? 'border-r-4 md:border-r-0 md:border-l-4' : 'border-l-4'} border-blue-600 mt-8 md:mt-0"
        >
          <h3 class="text-xl font-bold text-white">{item.title}</h3>
          <p class="text-gray-300 font-medium mb-2">{item.subtitle}</p>
          <p class="text-blue-400 text-sm mb-2 md:hidden">{item.year}</p>
          <p class="text-gray-400">{item.description}</p>
        </div>
      {/if}
    </div>
  </div>
</div>