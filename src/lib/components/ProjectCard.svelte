<script lang="ts">
  export interface Project {
    id: string;
    title: string;
    tagline: string;
    category: string;
    role: string;
    themeColor: 'cyan' | 'purple' | 'emerald';
    problem: string;
    solution: string;
    techStack: string[];
    impact: string[];
    link?: string;
    github?: string;
    images?: string[];
  }

  interface Props {
    project: Project;
  }

  let { project }: Props = $props();
  let activeTab = $state<'overview' | 'details' | 'impact'>('overview');
  let showLightbox = $state(false);
  let activeImageIndex = $state(0);

  const themeClasses = {
    cyan: {
      border: 'hover:border-brand-cyan/35 hover:shadow-[0_0_20px_rgba(6,182,212,0.08)]',
      text: 'text-brand-cyan',
      glow: 'rgba(6, 182, 212, 0.02)'
    },
    purple: {
      border: 'hover:border-brand-purple/35 hover:shadow-[0_0_20px_rgba(139,92,246,0.08)]',
      text: 'text-brand-purple',
      glow: 'rgba(139, 92, 246, 0.02)'
    },
    emerald: {
      border: 'hover:border-brand-emerald/35 hover:shadow-[0_0_20px_rgba(16,185,129,0.08)]',
      text: 'text-brand-emerald',
      glow: 'rgba(16, 185, 129, 0.02)'
    }
  };

  const currentTheme = $derived(themeClasses[project.themeColor]);

  function openLightbox() {
    showLightbox = true;
  }
  function closeLightbox() {
    showLightbox = false;
  }
  function nextImage(e: MouseEvent) {
    e.stopPropagation();
    if (!project.images) return;
    activeImageIndex = (activeImageIndex + 1) % project.images.length;
  }
  function prevImage(e: MouseEvent) {
    e.stopPropagation();
    if (!project.images) return;
    activeImageIndex = (activeImageIndex - 1 + project.images.length) % project.images.length;
  }
</script>

<div
  role="region"
  aria-label="Interactive project card"
  class="glass-panel relative w-full rounded-2xl p-5 md:p-6 transition-all duration-300 overflow-hidden flex flex-col justify-between {currentTheme.border} hover:-translate-y-1"
  style:background="linear-gradient(135deg, var(--card-bg) 0%, {currentTheme.glow} 100%)"
>
  <div>
    <!-- Card Header -->
    <div class="flex justify-between items-start gap-4 border-b border-border-primary pb-3 mb-4">
      <div>
        <div class="flex items-center gap-2 mb-1">
          <span class="text-[10px] font-mono uppercase tracking-wider text-text-secondary">{project.category}</span>
          <span class="w-1 h-1 rounded-full bg-border-primary"></span>
          <span class="text-[10px] font-mono uppercase tracking-wider text-text-secondary">{project.role}</span>
        </div>
        <h3 class="text-xl font-sans font-bold text-text-white tracking-tight">
          {project.title}
        </h3>
      </div>

      <!-- Links -->
      <div class="flex items-center gap-1.5 mt-1">
        {#if project.github}
          <a 
            href={project.github} 
            target="_blank" 
            rel="noopener noreferrer" 
            aria-label="GitHub Repository"
            class="w-7 h-7 rounded-lg border border-border-primary bg-bg-secondary hover:bg-btn-hover text-text-secondary hover:text-text-white flex items-center justify-center transition-all cursor-pointer shadow-sm"
          >
            <svg class="w-3.5 h-3.5" fill="currentColor" viewBox="0 0 24 24"><path d="M12 .297c-6.63 0-12 5.373-12 12 0 5.303 3.438 9.8 8.205 11.385.6.113.82-.258.82-.577 0-.285-.01-1.04-.015-2.04-3.338.724-4.042-1.61-4.042-1.61C4.422 18.07 3.633 17.7 3.633 17.7c-1.087-.744.084-.729.084-.729 1.205.084 1.838 1.236 1.838 1.236 1.07 1.835 2.809 1.305 3.495.998.108-.776.417-1.305.76-1.605-2.665-.3-5.466-1.332-5.466-5.93 0-1.31.465-2.38 1.235-3.22-.135-.303-.54-1.523.105-3.176 0 0 1.005-.322 3.3 1.23.96-.267 1.98-.399 3-.405 1.02.006 2.04.138 3 .405 2.28-1.552 3.285-1.23 3.285-1.23.645 1.653.24 2.873.12 3.176.765.84 1.23 1.91 1.23 3.22 0 4.61-2.805 5.625-5.475 5.92.42.36.81 1.096.81 2.22 0 1.606-.015 2.896-.015 3.286 0 .315.21.69.825.57C20.565 22.092 24 17.592 24 12.297c0-6.627-5.373-12-12-12"/></svg>
          </a>
        {/if}
        {#if project.link && project.link !== '#'}
          <a 
            href={project.link} 
            target="_blank" 
            rel="noopener noreferrer" 
            aria-label="Live Demo"
            class="w-7 h-7 rounded-lg border border-border-primary bg-bg-secondary hover:bg-btn-hover text-text-secondary hover:text-text-white flex items-center justify-center transition-all cursor-pointer shadow-sm"
          >
            <svg class="w-3.5 h-3.5 text-brand-cyan" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 6H6a2 2 0 00-2 2v10a2 2 0 002 2h10a2 2 0 002-2v-4M14 4h6m0 0v6m0-6L10 14"/></svg>
          </a>
        {/if}
      </div>
    </div>

    <!-- Tab Selectors -->
    <div class="flex border-b border-border-primary mb-4 text-[11px] font-mono gap-4">
      <button 
        onclick={() => activeTab = 'overview'} 
        class="pb-1.5 border-b-2 transition-all cursor-pointer {activeTab === 'overview' ? 'border-brand-cyan text-text-white font-semibold' : 'border-transparent text-text-muted hover:text-text-secondary'}"
      >
        Overview
      </button>
      <button 
        onclick={() => activeTab = 'details'} 
        class="pb-1.5 border-b-2 transition-all cursor-pointer {activeTab === 'details' ? 'border-brand-cyan text-text-white font-semibold' : 'border-transparent text-text-muted hover:text-text-secondary'}"
      >
        Inside the Code
      </button>
      <button 
        onclick={() => activeTab = 'impact'} 
        class="pb-1.5 border-b-2 transition-all cursor-pointer {activeTab === 'impact' ? 'border-brand-cyan text-text-white font-semibold' : 'border-transparent text-text-muted hover:text-text-secondary'}"
      >
        Impact
      </button>
    </div>

    <!-- Tab Content Container with Fade Transition -->
    <div class="min-h-[160px] flex flex-col justify-between">
      {#if activeTab === 'overview'}
        <div class="space-y-4 animate-fadeIn">
          <!-- Screenshot Slideshow / Gallery -->
          {#if project.images && project.images.length > 0}
            <div class="relative group/gallery overflow-hidden rounded-xl border border-border-primary bg-bg-secondary h-[150px] sm:h-[160px]">
              <!-- Active Image button wrapper for accessibility -->
              <button 
                type="button"
                class="w-full h-full p-0 border-0 outline-none cursor-zoom-in overflow-hidden rounded-xl block focus:ring-1 focus:ring-brand-cyan/30"
                onclick={openLightbox}
              >
                <img 
                  src={project.images[activeImageIndex]} 
                  alt={`${project.title} screenshot`} 
                  class="w-full h-full object-cover transition-all duration-500 hover:scale-105"
                />
              </button>
              
              <!-- Gallery Navigation Arrows (if multiple images) -->
              {#if project.images.length > 1}
                <button 
                  onclick={prevImage} 
                  class="absolute left-2 top-1/2 -translate-y-1/2 w-6 h-6 rounded-full bg-black/60 text-white flex items-center justify-center hover:bg-black/80 transition-all opacity-0 group-hover/gallery:opacity-100 cursor-pointer text-xs"
                  aria-label="Previous screenshot"
                >
                  &larr;
                </button>
                <button 
                  onclick={nextImage} 
                  class="absolute right-2 top-1/2 -translate-y-1/2 w-6 h-6 rounded-full bg-black/60 text-white flex items-center justify-center hover:bg-black/80 transition-all opacity-0 group-hover/gallery:opacity-100 cursor-pointer text-xs"
                  aria-label="Next screenshot"
                >
                  &rarr;
                </button>
                
                <!-- Dot indicators -->
                <div class="absolute bottom-2 left-1/2 -translate-x-1/2 flex gap-1 bg-black/40 px-2 py-1 rounded-full">
                  {#each project.images as _, idx}
                    <button 
                      onclick={() => activeImageIndex = idx}
                      class="w-1.5 h-1.5 rounded-full transition-all cursor-pointer {activeImageIndex === idx ? 'bg-brand-cyan scale-110' : 'bg-white/45'}"
                      aria-label={`Go to screenshot ${idx + 1}`}
                    ></button>
                  {/each}
                </div>
              {/if}
            </div>
          {/if}

          <p class="text-text-primary text-xs md:text-sm leading-relaxed">
            {project.tagline}
          </p>
          
          <!-- Tech tags -->
          <div>
            <span class="block text-[10px] font-mono text-text-muted uppercase mb-1.5">Stack & Technologies</span>
            <div class="flex flex-wrap gap-1.5">
              {#each project.techStack as tech}
                <span class="px-2 py-0.5 rounded text-[9px] font-mono border border-border-primary bg-bg-secondary text-text-secondary">
                  {tech}
                </span>
              {/each}
            </div>
          </div>
        </div>
      {:else if activeTab === 'details'}
        <div class="space-y-3 p-3.5 bg-bg-secondary/40 border border-border-primary rounded-xl text-xs animate-fadeIn">
          <!-- Challenge Row -->
          <div class="flex flex-col gap-1">
            <span class="text-red-400 font-mono text-[9px] font-bold uppercase tracking-wider">Challenge</span>
            <p class="text-text-secondary leading-relaxed">{project.problem}</p>
          </div>

          <!-- Action Row -->
          <div class="flex flex-col gap-1 border-t border-border-primary pt-2.5">
            <span class="text-emerald-400 font-mono text-[9px] font-bold uppercase tracking-wider">Solution</span>
            <p class="text-text-secondary leading-relaxed">{project.solution}</p>
          </div>
        </div>
      {:else if activeTab === 'impact'}
        <div class="space-y-3 p-3.5 bg-bg-secondary/40 border border-border-primary rounded-xl text-xs animate-fadeIn">
          <span class="text-brand-cyan font-mono text-[9px] font-bold uppercase tracking-wider block mb-1">Key Outcomes</span>
          <ul class="space-y-2 text-text-primary leading-relaxed">
            {#each project.impact as impactItem}
              <li class="flex items-start gap-2">
                <span class="text-brand-cyan select-none font-bold">✓</span>
                <span>{impactItem}</span>
              </li>
            {/each}
          </ul>
        </div>
      {/if}
    </div>
  </div>
</div>

<!-- Lightbox modal portal -->
{#if showLightbox && project.images}
  <div 
    class="fixed inset-0 z-50 bg-black/95 backdrop-blur-md flex flex-col items-center justify-center p-4 animate-fadeInFast"
    onclick={closeLightbox}
    onkeydown={(e) => e.key === 'Escape' && closeLightbox()}
    role="dialog"
    aria-modal="true"
    tabindex="-1"
  >
    <!-- Close button -->
    <button 
      onclick={closeLightbox} 
      class="absolute top-6 right-6 text-white/70 hover:text-white text-3xl font-sans cursor-pointer focus:outline-none bg-white/5 w-10 h-10 rounded-full flex items-center justify-center border border-white/10 hover:bg-white/15"
      aria-label="Close image viewer"
    >
      ×
    </button>
    
    <!-- Image Wrapper -->
    <!-- svelte-ignore a11y_click_events_have_key_events -->
    <!-- svelte-ignore a11y_no_static_element_interactions -->
    <div class="relative max-w-5xl max-h-[80vh] flex items-center justify-center" onclick={(e) => e.stopPropagation()}>
      <img 
        src={project.images[activeImageIndex]} 
        alt={`${project.title} full view`} 
        class="max-w-full max-h-[80vh] object-contain rounded-lg shadow-2xl border border-white/5"
      />
      
      <!-- Slideshow arrows in Lightbox -->
      {#if project.images.length > 1}
        <button 
          onclick={prevImage} 
          class="absolute left-4 top-1/2 -translate-y-1/2 w-12 h-12 rounded-full bg-black/60 border border-white/10 text-white flex items-center justify-center hover:bg-white/40 transition-all cursor-pointer text-xl"
          aria-label="Previous screenshot"
        >
          &#x2190;
        </button>
        <button 
          onclick={nextImage} 
          class="absolute right-4 top-1/2 -translate-y-1/2 w-12 h-12 rounded-full bg-black/60 border border-white/10 text-white flex items-center justify-center hover:bg-white/40 transition-all cursor-pointer text-xl"
          aria-label="Next screenshot"
        >
          &#x2192;
        </button>
      {/if}
    </div>

    <!-- Title and counter at bottom -->
    <div class="mt-4 text-center font-mono text-xs text-white/60">
      <span class="text-white font-bold">{project.title}</span> — {activeImageIndex + 1} of {project.images.length}
    </div>
  </div>
{/if}

<style>
  @keyframes fadeIn {
    from {
      opacity: 0;
      transform: translateY(4px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }
  @keyframes fadeInFast {
    from { opacity: 0; }
    to { opacity: 1; }
  }
  .animate-fadeIn {
    animation: fadeIn 0.25s cubic-bezier(0.16, 1, 0.3, 1) forwards;
  }
  .animate-fadeInFast {
    animation: fadeInFast 0.15s ease-out forwards;
  }
</style>
