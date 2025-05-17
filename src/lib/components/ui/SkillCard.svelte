<script>
  import { spring } from 'svelte/motion';
  
  export let skill;
  
  // Animation for card hover effect
  const scale = spring(1, {
    stiffness: 0.2,
    damping: 0.8
  });
  
  function handleMouseEnter() {
    scale.set(1.05);
  }
  
  function handleMouseLeave() {
    scale.set(1);
  }
  
  // Category color mapping
  const getCategoryColor = (category) => {
    const colors = {
      'frontend': 'from-blue-500 to-indigo-600',
      'backend': 'from-green-500 to-teal-600',
      'database': 'from-orange-500 to-amber-600',
      'devops': 'from-red-500 to-rose-600',
      'other': 'from-purple-500 to-fuchsia-600'
    };
    
    return colors[category] || 'from-gray-500 to-gray-600';
  };
</script>

<div 
  class="h-36 rounded-xl p-0.5 shadow-lg transition-all duration-300"
  style="transform: scale({$scale}); background: linear-gradient(to bottom right, var(--from), var(--to));"
  style:--from="var(--tw-gradient-from)"
  style:--to="var(--tw-gradient-to)"
  class:bg-gradient-to-br={true}
  class:{getCategoryColor(skill.category)}={true}
  on:mouseenter={handleMouseEnter}
  on:mouseleave={handleMouseLeave}
>
  <div class="bg-gray-800 h-full w-full rounded-lg flex flex-col items-center justify-center p-4 relative overflow-hidden">
    <!-- Skill Icon -->
    {#if skill.icon}
      <div class="text-2xl mb-2 {skill.iconColor || 'text-white'}">
        {@html skill.icon}
      </div>
    {/if}
    
    <!-- Skill Name -->
    <h3 class="text-lg font-semibold text-white mb-2">{skill.name}</h3>
    
    <!-- Progress Bar -->
    <div class="w-full bg-gray-700 rounded-full h-2.5 overflow-hidden">
      <div 
        class="h-2.5 rounded-full bg-gradient-to-r {getCategoryColor(skill.category)}"
        style="width: {skill.level}%"
      ></div>
    </div>
    
    <!-- Hover effect overlay -->
    <div class="absolute inset-0 bg-gradient-to-tr from-blue-600/20 to-purple-600/0 opacity-0 transition-opacity duration-300 hover:opacity-100"></div>
  </div>
</div>