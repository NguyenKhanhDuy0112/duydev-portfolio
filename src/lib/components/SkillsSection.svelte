<script>
  import { onMount } from 'svelte';
  import { fly } from 'svelte/transition';
  import { quintOut } from 'svelte/easing';
  import { skills } from '$lib/data/skills';
  import SkillCard from '$lib/components/ui/SkillCard.svelte';
  import { intersectionObserver } from '$lib/utils/intersectionObserver';
  
  let visible = false;
  let selectedCategory = 'all';
  
  $: filteredSkills = selectedCategory === 'all' 
    ? skills 
    : skills.filter(skill => skill.category === selectedCategory);
  
  const categories = [
    { id: 'all', name: 'All Skills' },
    { id: 'frontend', name: 'Frontend' },
    { id: 'backend', name: 'Backend' },
    { id: 'database', name: 'Database' },
    { id: 'devops', name: 'DevOps' }
  ];
  
  onMount(() => {
    const section = document.getElementById('skills');
    
    if (section) {
      intersectionObserver(section, () => {
        visible = true;
      });
    }
  });
</script>

<section id="skills" class="py-24 relative">
  <!-- Background Gradient -->
  <div class="absolute inset-0 bg-gradient-to-b from-gray-900 via-gray-900 to-gray-800 -z-10"></div>
  
  <div class="container mx-auto px-6">
    <!-- Section Title -->
    {#if visible}
      <div 
        class="text-center mb-16" 
        in:fly={{ y: -20, duration: 800, delay: 100 }}
      >
        <h2 class="text-4xl md:text-5xl font-bold text-white mb-4">
          Technical Skills
        </h2>
        <div class="w-24 h-1 bg-gradient-to-r from-blue-500 to-purple-500 mx-auto"></div>
      </div>
      
      <!-- Category Filter -->
      <div 
        class="flex flex-wrap justify-center mb-12 gap-2" 
        in:fly={{ y: 20, duration: 800, delay: 200 }}
      >
        {#each categories as category}
          <button 
            class="px-4 py-2 rounded-md transition-all duration-300 {selectedCategory === category.id 
              ? 'bg-gradient-to-r from-blue-600 to-purple-600 text-white' 
              : 'bg-gray-800 text-gray-400 hover:text-white hover:bg-gray-700'}"
            on:click={() => selectedCategory = category.id}
          >
            {category.name}
          </button>
        {/each}
      </div>
      
      <!-- Skills Grid -->
      <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-6">
        {#each filteredSkills as skill, i}
          <div in:fly={{ 
            y: 40, 
            duration: 600, 
            delay: 300 + (i * 50), 
            easing: quintOut 
          }}>
            <SkillCard {skill} />
          </div>
        {/each}
      </div>
    {/if}
  </div>
</section>