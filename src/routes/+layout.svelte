<script lang="ts">
  import './layout.css';
  import ThreeBackground from '$lib/components/ThreeBackground.svelte';

  let { children } = $props();
</script>

<svelte:head>
  <title>Muhammad Jawahiruzzaman | Software Engineer</title>
  <meta name="description" content="Portfolio of Muhammad Jawahiruzzaman, a professional Software Engineer specializing in building scalable healthcare and business platforms." />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  
  <!-- SEO Open Graph -->
  <meta property="og:title" content="Muhammad Jawahiruzzaman | Software Engineer" />
  <meta property="og:description" content="Professional Software Engineer with experience in healthcare EMR and SaaS marketplace platforms. Focused on impact, performance, and collaboration." />
  <meta property="og:type" content="website" />
  <meta name="twitter:card" content="summary_large_image" />
</svelte:head>

<!-- Core Layout Shell -->
<div class="relative min-h-screen text-white bg-dark-bg selection:bg-brand-cyan/30 selection:text-white overflow-x-hidden">
  
  <!-- WebGL Particles background -->
  <ThreeBackground />

  <!-- Floating Glowing Grid Lines Background -->
  <div class="absolute inset-0 -z-10 pointer-events-none cyber-grid cyber-grid-radial opacity-50"></div>
  <div class="absolute inset-0 -z-10 pointer-events-none cyber-noise"></div>

  <!-- Header Navigation -->
  <header class="fixed top-5 left-1/2 -translate-x-1/2 w-[92%] max-w-5xl z-40">
    <nav class="glass-panel rounded-full px-5 py-3 md:px-8 flex items-center justify-between shadow-[0_12px_40px_rgba(0,0,0,0.65)] hover:border-white/15 transition-colors duration-300">
      
      <!-- Brand Logo -->
      <a href="#hero" class="flex items-center gap-2 group">
        <span class="font-mono font-extrabold text-sm md:text-base tracking-wider text-white">
          JAWAHIR<span class="text-brand-cyan group-hover:animate-pulse">_</span>
        </span>
        <span class="hidden sm:inline-block text-[9px] font-mono px-2 py-0.5 rounded border border-white/10 bg-white/5 text-neutral-400">
          ENGINEER
        </span>
      </a>

      <!-- Mid Nav Links -->
      <div class="flex items-center gap-3 sm:gap-6 md:gap-8 font-mono text-[10px] sm:text-xs md:text-sm">
        <a href="#projects" class="text-neutral-400 hover:text-white transition-colors">Projects</a>
        <a href="#skills" class="text-neutral-400 hover:text-white transition-colors">Tech</a>
        <a href="#experience" class="text-neutral-400 hover:text-white transition-colors">History</a>
      </div>

      <!-- Action Status (Indicator) -->
      <div class="flex items-center gap-1.5 md:gap-2">
        <span class="relative flex h-2 w-2">
          <span class="animate-ping absolute inline-flex h-full w-full rounded-full bg-brand-emerald opacity-75"></span>
          <span class="relative inline-flex rounded-full h-2 w-2 bg-brand-emerald"></span>
        </span>
        <a href="#contact" class="text-[9px] md:text-xs font-mono text-neutral-300 hover:text-brand-cyan tracking-wider font-semibold transition-colors">
          <span class="inline sm:hidden">OPEN</span>
          <span class="hidden sm:inline">OPEN_FOR_ROLE</span>
        </a>
      </div>

    </nav>
  </header>

  <!-- Content Slot -->
  <main class="w-full relative z-10 pt-24">
    {@render children()}
  </main>

  <!-- Global Footer -->
  <footer class="w-full border-t border-white/5 bg-black/45 py-12 relative z-10 text-center font-mono text-xs text-neutral-500">
    <div class="max-w-7xl mx-auto px-4 flex flex-col md:flex-row justify-between items-center gap-6">
      <div class="flex items-center gap-2">
        <span>© {new Date().getFullYear()} Muhammad Jawahiruzzaman.</span>
        <span class="text-neutral-700">|</span>
        <span class="text-brand-cyan text-[10px]">DESIGNED_FOR_PRODUCTION</span>
      </div>
      
      <div class="flex gap-6">
        <a href="https://github.com" target="_blank" rel="noopener noreferrer" class="hover:text-white transition-colors">GitHub</a>
        <a href="https://linkedin.com" target="_blank" rel="noopener noreferrer" class="hover:text-white transition-colors">LinkedIn</a>
        <a href="mailto:m.jawahiruzzaman@gmail.com" class="hover:text-white transition-colors">Email</a>
      </div>
    </div>
  </footer>

</div>
