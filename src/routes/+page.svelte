<!-- src/routes/+page.svelte -->
<script>
  import HeroSection from '$lib/components/HeroSection.svelte';
  import AboutSection from '$lib/components/AboutSection.svelte';
  import SkillsSection from '$lib/components/SkillsSection.svelte';
  import ProjectsSection from '$lib/components/ProjectsSection.svelte';
  import ExperienceSection from '$lib/components/ExperienceSection.svelte';
  import ContactSection from '$lib/components/ContactSection.svelte';
  
  // Xử lý mịn cho scroll behavior
  import { onMount } from 'svelte';
  
  onMount(() => {
    const links = document.querySelectorAll('a[href^="#"]');
    
    links.forEach(link => {
      link.addEventListener('click', function(e) {
        e.preventDefault();
        
        const targetId = this.getAttribute('href').substring(1);
        const targetElement = document.getElementById(targetId);
        
        if (targetElement) {
          window.scrollTo({
            top: targetElement.offsetTop - 80, // Offset for header
            behavior: 'smooth'
          });
        }
      });
    });
  });
</script>

<svelte:head>
  <title>Duy Nguyen | Fullstack Developer</title>
  <meta name="description" content="Duy Nguyen - Fullstack Developer with expertise in ReactJS, NextJS, SvelteKit, NestJS and more" />
</svelte:head>

<main>
  <HeroSection />
  <AboutSection />
  <SkillsSection />
  <ProjectsSection />
  <ExperienceSection />
  <ContactSection />
</main>

<style lang="postcss">
  :global(html) {
    scroll-behavior: smooth;
  }
  
  :global(body) {
    @apply bg-gray-900 text-gray-100 overflow-x-hidden;
    font-family: 'Inter', sans-serif;
  }
  
  main {
    @apply flex flex-col min-h-screen;
  }
</style>