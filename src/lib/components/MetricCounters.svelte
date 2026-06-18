<script lang="ts">
  import { onMount } from 'svelte';

  interface Metric {
    value: number;
    suffix: string;
    label: string;
    sublabel: string;
    glowColor: string;
  }

  const metrics: Metric[] = [
    { value: 3, suffix: '+', label: 'Years of Experience', sublabel: 'Professional software engineering', glowColor: 'text-glow-cyan' },
    { value: 4, suffix: '', label: 'Professional Roles', sublabel: 'Healthcare, SaaS, & Game dev', glowColor: 'text-glow-purple' },
    { value: 5, suffix: '+', label: 'Production Systems', sublabel: 'Maintained EMR, SaaS, & mobile games', glowColor: 'text-glow-green' },
    { value: 2, suffix: '', label: 'HackerRank Badges', sublabel: 'JavaScript & Problem Solving', glowColor: 'text-glow-purple' }
  ];

  let displayValues = $state(metrics.map(() => 0));
  let element: HTMLElement;

  onMount(() => {
    if (!element) return;

    const observer = new IntersectionObserver((entries) => {
      if (entries[0].isIntersecting) {
        animateCounters();
        observer.disconnect();
      }
    }, { threshold: 0.1 });

    observer.observe(element);
    return () => observer.disconnect();
  });

  function animateCounters() {
    metrics.forEach((metric, index) => {
      const end = metric.value;
      const duration = 1800; // ms
      let startTime: number | null = null;

      function step(timestamp: number) {
        if (!startTime) startTime = timestamp;
        const progress = Math.min((timestamp - startTime) / duration, 1);
        
        // Easing: easeOutExpo for a premium organic deceleration
        const ease = progress === 1 ? 1 : 1 - Math.pow(2, -10 * progress);
        displayValues[index] = Math.floor(ease * end);

        if (progress < 1) {
          requestAnimationFrame(step);
        } else {
          displayValues[index] = end;
        }
      }
      requestAnimationFrame(step);
    });
  }
</script>

<div bind:this={element} class="grid grid-cols-2 md:grid-cols-4 gap-4 md:gap-6 my-12 w-full max-w-7xl mx-auto px-4">
  {#each metrics as metric, i}
    <div class="glass-panel relative overflow-hidden rounded-2xl p-5 md:p-6 flex flex-col justify-between group transition-all duration-300 hover:border-brand-cyan/30 hover:shadow-[0_0_20px_rgba(6,182,212,0.1)]">
      <!-- Glow effect on hover -->
      <div class="absolute -right-16 -top-16 w-32 h-32 rounded-full bg-brand-cyan/5 blur-2xl group-hover:bg-brand-cyan/10 transition-all duration-500"></div>
      
      <div>
        <div class="flex items-baseline font-mono text-4xl md:text-5xl font-extrabold tracking-tight text-text-white mb-2">
          <span class="bg-gradient-to-r from-text-white via-text-primary to-text-secondary bg-clip-text text-transparent group-hover:from-brand-cyan group-hover:to-brand-purple transition-all duration-500">
            {displayValues[i]}
          </span>
          {#if metric.suffix}
            <span class="text-brand-cyan ml-1 text-2xl md:text-3xl">{metric.suffix}</span>
          {/if}
        </div>
        
        <h4 class="text-text-white font-medium text-sm md:text-base leading-tight tracking-wide mb-1 font-sans">
          {metric.label}
        </h4>
      </div>

      <p class="text-text-muted text-xs leading-normal mt-2 border-t border-border-primary pt-2 font-mono">
        {metric.sublabel}
      </p>
    </div>
  {/each}
</div>
