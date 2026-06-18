<script lang="ts">
  import { onMount } from 'svelte';
  import './layout.css';
  import ThreeBackground from '$lib/components/ThreeBackground.svelte';

  let { children } = $props();
  let isLight = $state(false);

  function toggleTheme() {
    isLight = !isLight;
    if (typeof window !== 'undefined') {
      localStorage.setItem('theme', isLight ? 'light' : 'dark');
      if (isLight) {
        document.documentElement.classList.add('light');
      } else {
        document.documentElement.classList.remove('light');
      }
    }
  }

  onMount(() => {
    if (typeof window !== 'undefined') {
      const savedTheme = localStorage.getItem('theme');
      const prefersLight = window.matchMedia('(prefers-color-scheme: light)').matches;
      isLight = savedTheme === 'light' || (!savedTheme && prefersLight);
      if (isLight) {
        document.documentElement.classList.add('light');
      } else {
        document.documentElement.classList.remove('light');
      }
    }
  });
</script>

<svelte:head>
  <title>Muhammad Jawahiruzzaman | Software Engineer</title>
  <meta name="description" content="Portfolio of Muhammad Jawahiruzzaman, a professional Software Engineer specializing in building scalable healthcare and business platforms." />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  
  <!-- Devicon stylesheet for brand logos -->
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/devicon.min.css" />
  
  <!-- SEO Open Graph -->
  <meta property="og:title" content="Muhammad Jawahiruzzaman | Software Engineer" />
  <meta property="og:description" content="Professional Software Engineer with experience in healthcare EMR and SaaS marketplace platforms. Focused on impact, performance, and collaboration." />
  <meta property="og:type" content="website" />
  <meta name="twitter:card" content="summary_large_image" />
</svelte:head>

<!-- Core Layout Shell -->
<div class="relative min-h-screen text-text-primary bg-bg-primary selection:bg-brand-cyan/30 selection:text-text-white overflow-x-hidden transition-colors duration-300">
  
  <!-- WebGL Particles background -->
  <ThreeBackground {isLight} />

  <!-- Floating Glowing Grid Lines Background -->
  <div class="absolute inset-0 -z-10 pointer-events-none cyber-grid cyber-grid-radial opacity-50"></div>
  <div class="absolute inset-0 -z-10 pointer-events-none cyber-noise"></div>

  <!-- Header Navigation -->
  <header class="fixed top-5 left-1/2 -translate-x-1/2 w-[92%] max-w-5xl z-40">
    <nav 
      class="glass-panel rounded-full px-5 py-3 md:px-8 flex items-center justify-between transition-all duration-300 hover:border-brand-cyan/20"
      style:box-shadow="var(--nav-shadow)"
    >
      
      <!-- Brand Logo -->
      <a href="#hero" class="flex items-center gap-2 group">
        <span class="font-mono font-extrabold text-sm md:text-base tracking-wider text-text-white">
          JAWAHIR<span class="text-brand-cyan group-hover:animate-pulse">_</span>
        </span>
        <span class="hidden sm:inline-block text-[9px] font-mono px-2 py-0.5 rounded border border-border-primary bg-bg-secondary text-text-secondary">
          ENGINEER
        </span>
      </a>

      <!-- Mid Nav Links -->
      <div class="flex items-center gap-3 sm:gap-6 md:gap-8 font-mono text-[10px] sm:text-xs md:text-sm">
        <a href="#projects" class="text-text-secondary hover:text-text-white transition-colors">Projects</a>
        <a href="#skills" class="text-text-secondary hover:text-text-white transition-colors">Tech</a>
        <a href="#experience" class="text-text-secondary hover:text-text-white transition-colors">History</a>
      </div>

      <!-- Action Status & Theme Toggle -->
      <div class="flex items-center gap-3.5">
        <div class="flex items-center gap-1.5 md:gap-2">
          <span class="relative flex h-2 w-2">
            <span class="animate-ping absolute inline-flex h-full w-full rounded-full bg-brand-emerald opacity-75"></span>
            <span class="relative inline-flex rounded-full h-2 w-2 bg-brand-emerald"></span>
          </span>
          <a href="#contact" class="text-[9px] md:text-xs font-mono text-text-secondary hover:text-brand-cyan tracking-wider font-semibold transition-colors">
            <span class="inline sm:hidden">OPEN</span>
            <span class="hidden sm:inline">OPEN_FOR_ROLE</span>
          </a>
        </div>

        <!-- Theme Toggle Button -->
        <button 
          onclick={toggleTheme} 
          aria-label="Toggle dark/light theme" 
          class="w-7 h-7 rounded-full border border-border-primary bg-bg-secondary hover:bg-btn-hover text-text-secondary hover:text-brand-cyan flex items-center justify-center transition-all cursor-pointer relative overflow-hidden group shadow-[inset_0_1px_0_rgba(255,255,255,0.05)] hover:scale-105 active:scale-95"
        >
          <!-- Sun Icon (visible in light mode) -->
          <svg class="w-3.5 h-3.5 transition-all duration-300 transform {isLight ? 'scale-100 rotate-0 opacity-100' : 'scale-0 rotate-90 opacity-0'} absolute" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 3v1m0 16v1m9-9h-1M4 12H3m15.364-6.364l-.707.707M6.343 17.657l-.707.707m0-12.728l.707.707m12.728 12.728l.707.707M12 8a4 4 0 100 8 4 4 0 000-8z" />
          </svg>
          <!-- Moon Icon (visible in dark mode) -->
          <svg class="w-3.5 h-3.5 transition-all duration-300 transform {isLight ? 'scale-0 -rotate-90 opacity-0' : 'scale-100 rotate-0 opacity-100'} absolute" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M20.354 15.354A9 9 0 018.646 3.646 9.003 9.003 0 0012 21a9.003 9.003 0 008.354-5.646z" />
          </svg>
        </button>
      </div>

    </nav>
  </header>

  <!-- Content Slot -->
  <main class="w-full relative z-10 pt-24">
    {@render children()}
  </main>

  <!-- Global Footer -->
  <footer class="w-full border-t border-border-primary bg-bg-secondary/40 py-12 relative z-10 text-center font-mono text-xs text-text-muted">
    <div class="max-w-7xl mx-auto px-4 flex flex-col md:flex-row justify-between items-center gap-6">
      <div class="flex items-center gap-2">
        <span>© {new Date().getFullYear()} Muhammad Jawahiruzzaman.</span>
        <span class="text-border-primary">|</span>
        <span class="text-brand-cyan text-[10px]">DESIGNED_FOR_PRODUCTION</span>
      </div>
      
      <div class="flex gap-6">
        <a href="https://github.com" target="_blank" rel="noopener noreferrer" class="hover:text-text-white transition-colors text-text-secondary">GitHub</a>
        <a href="https://linkedin.com" target="_blank" rel="noopener noreferrer" class="hover:text-text-white transition-colors text-text-secondary">LinkedIn</a>
        <a href="mailto:m.jawahiruzzaman@gmail.com" class="hover:text-text-white transition-colors text-text-secondary">Email</a>
      </div>
    </div>
  </footer>

</div>
