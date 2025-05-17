<script>
  import { onMount, afterUpdate } from 'svelte';
  import { spring } from 'svelte/motion';
  import { fade, fly, scale } from 'svelte/transition';
  
  export let project;
  export let active = false;
  
  let perspective = spring(0, {
    stiffness: 0.1,
    damping: 0.8
  });
  
  let cardElement;
  let isInView = false;
  let mouseX = 0;
  let mouseY = 0;
  
  // Create array of technology badges from the comma-separated string
  $: technologies = project.technologies ? project.technologies.split(',').map(tech => tech.trim()) : [];
  
  const getTechBadgeColor = (tech) => {
    const techColors = {
      'ReactJS': 'bg-blue-600',
      'NextJS': 'bg-gray-800',
      'SvelteKit': 'bg-orange-600',
      'NestJS': 'bg-red-600',
      'ElectronJS': 'bg-blue-500',
      'Laravel': 'bg-red-500',
      'PostgreSQL': 'bg-blue-700',
      'Redis': 'bg-red-700',
      'RabbitMQ': 'bg-orange-800',
      'Docker': 'bg-blue-800',
      'VPS': 'bg-gray-800',
      'GitHub Actions': 'bg-purple-800',
      'Turborepo': 'bg-emerald-700',
    };
    
    return techColors[tech] || 'bg-gray-700';
  };
  
  function handleMouseMove(event) {
    if (!cardElement || !active) return;
    
    const rect = cardElement.getBoundingClientRect();
    
    // Calculate mouse position relative to the card
    mouseX = (event.clientX - rect.left) / rect.width;
    mouseY = (event.clientY - rect.top) / rect.height;
    
    // Update perspective spring
    perspective.set(1);
  }
  
  function handleMouseLeave() {
    perspective.set(0);
  }
  
  afterUpdate(() => {
    // Reset perspective when card becomes inactive
    if (!active) {
      perspective.set(0);
    }
  });
  
  onMount(() => {
    isInView = true;
  });
</script>

<div 
  bind:this={cardElement}
  class="relative rounded-xl overflow-hidden transition-all duration-500"
  class:shadow-2xl={active}
  class:shadow-blue-600/20={active}
  class:opacity-100={active}
  class:opacity-40={!active}
  style="transform: perspective(1000px) rotateY({$perspective * (mouseX - 0.5) * 10}deg) rotateX({$perspective * (mouseY - 0.5) * -10}deg)"
  on:mousemove={handleMouseMove}
  on:mouseleave={handleMouseLeave}
>
  <!-- Project Image -->
  <div class="relative h-80 w-full overflow-hidden rounded-t-xl">
    <img 
      src={project.image || `/projects/${project.title.toLowerCase().replace(/\s+/g, '-')}.jpg`}
      alt={project.title}
      class="w-full h-full object-cover transition-transform duration-700 hover:scale-105"
    />
    <div class="absolute inset-0 bg-gradient-to-t from-gray-900 via-gray-900/40 to-transparent opacity-90"></div>
    
    <!-- Project Title Overlay -->
    <div class="absolute bottom-0 left-0 right-0 p-6">
      <h3 class="text-2xl sm:text-3xl font-bold text-white mb-2">{project.title}</h3>
      
      <!-- Technology Badges -->
      <div class="flex flex-wrap gap-2 mb-4">
        {#each technologies as tech}
          <span class={`text-xs font-semibold px-2 py-1 rounded ${getTechBadgeColor(tech)} text-white`}>
            {tech}
          </span>
        {/each}
      </div>
    </div>
  </div>
  
  <!-- Project Details -->
  <div class="p-6 bg-gray-800">
    <p class="text-gray-300 mb-6">{project.description}</p>
    
    <!-- Project Links -->
    <div class="flex space-x-4">
      {#if project.projectUrl}
        <a 
          href={project.projectUrl} 
          target="_blank" 
          rel="noopener noreferrer"
          class="flex-1 text-center py-2 px-4 bg-gradient-to-r from-blue-600 to-indigo-600 text-white rounded-md hover:from-blue-700 hover:to-indigo-700 transition-colors duration-300 font-medium"
        >
          View Project
        </a>
      {/if}
      
      {#if project.githubUrl}
        <a 
          href={project.githubUrl} 
          target="_blank" 
          rel="noopener noreferrer"
          class="flex-1 text-center py-2 px-4 bg-gray-700 text-white rounded-md hover:bg-gray-600 transition-colors duration-300 font-medium"
        >
          <span class="flex justify-center items-center">
            <svg class="w-5 h-5 mr-2" fill="currentColor" viewBox="0 0 24 24" aria-hidden="true">
              <path fill-rule="evenodd" d="M12 2C6.477 2 2 6.484 2 12.017c0 4.425 2.865 8.18 6.839 9.504.5.092.682-.217.682-.483 0-.237-.008-.868-.013-1.703-2.782.605-3.369-1.343-3.369-1.343-.454-1.158-1.11-1.466-1.11-1.466-.908-.62.069-.608.069-.608 1.003.07 1.531 1.032 1.531 1.032.892 1.53 2.341 1.088 2.91.832.092-.647.35-1.088.636-1.338-2.22-.253-4.555-1.113-4.555-4.951 0-1.093.39-1.988 1.029-2.688-.103-.253-.446-1.272.098-2.65 0 0 .84-.27 2.75 1.026A9.564 9.564 0 0112 6.844c.85.004 1.705.115 2.504.337 1.909-1.296 2.747-1.027 2.747-1.027.546 1.379.202 2.398.1 2.651.64.7 1.028 1.595 1.028 2.688 0 3.848-2.339 4.695-4.566 4.943.359.309.678.92.678 1.855 0 1.338-.012 2.419-.012 2.747 0 .268.18.58.688.482A10.019 10.019 0 0022 12.017C22 6.484 17.522 2 12 2z" clip-rule="evenodd"></path>
            </svg>
            Source Code
          </span>
        </a>
      {/if}
    </div>
  </div>
  
  <!-- 3D Light effect overlay -->
  {#if active && $perspective > 0}
    <div 
      class="absolute inset-0 pointer-events-none"
      style="background: radial-gradient(circle at {mouseX * 100}% {mouseY * 100}%, rgba(191, 219, 254, 0.3) 0%, rgba(30, 64, 175, 0) 60%)"
    ></div>
  {/if}
</div>