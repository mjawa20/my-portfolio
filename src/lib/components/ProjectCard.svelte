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
  }

  interface Props {
    project: Project;
  }

  let { project }: Props = $props();

  const themeClasses = {
    cyan: {
      border: 'hover:border-brand-cyan/35',
      text: 'text-brand-cyan',
      iconBg: 'bg-brand-cyan/10 border-brand-cyan/20 text-brand-cyan',
      glow: 'rgba(6, 182, 212, 0.05)'
    },
    purple: {
      border: 'hover:border-brand-purple/35',
      text: 'text-brand-purple',
      iconBg: 'bg-brand-purple/10 border-brand-purple/20 text-brand-purple',
      glow: 'rgba(139, 92, 246, 0.05)'
    },
    emerald: {
      border: 'hover:border-brand-emerald/35',
      text: 'text-brand-emerald',
      iconBg: 'bg-brand-emerald/10 border-brand-emerald/20 text-brand-emerald',
      glow: 'rgba(16, 185, 129, 0.05)'
    }
  };

  const currentTheme = $derived(themeClasses[project.themeColor]);
</script>

<div
  role="region"
  aria-label="Interactive project card"
  class="glass-panel relative w-full rounded-2xl p-5 md:p-6 transition-all duration-300 overflow-hidden {currentTheme.border} hover:-translate-y-1"
  style:background="linear-gradient(135deg, rgba(10, 10, 12, 0.7) 0%, {currentTheme.glow} 100%)"
>
  <!-- Card Header -->
  <div class="flex justify-between items-start gap-4 border-b border-white/5 pb-3 mb-4">
    <div>
      <div class="flex items-center gap-2 mb-1">
        <span class="text-[10px] font-mono uppercase tracking-wider text-neutral-400">{project.category}</span>
        <span class="w-1 h-1 rounded-full bg-white/20"></span>
        <span class="text-[10px] font-mono uppercase tracking-wider text-neutral-400">{project.role}</span>
      </div>
      <h3 class="text-xl font-sans font-bold text-white tracking-tight">
        {project.title}
      </h3>
    </div>
  </div>

  <!-- Description -->
  <p class="text-neutral-300 text-xs md:text-sm leading-relaxed mb-4">
    {project.tagline}
  </p>

  <!-- Challenge -> Action -> Outcomes Pipeline (Compact & Clean) -->
  <div class="space-y-3 bg-neutral-950/40 border border-white/5 p-4 rounded-xl text-xs">
    
    <!-- Challenge Row -->
    <div class="flex items-start gap-3">
      <span class="text-red-400 font-mono text-[10px] font-bold mt-0.5 uppercase tracking-wide min-w-[70px]">Challenge</span>
      <p class="text-neutral-400 leading-normal">{project.problem}</p>
    </div>

    <!-- Action Row -->
    <div class="flex items-start gap-3 border-t border-white/5 pt-2">
      <span class="text-emerald-400 font-mono text-[10px] font-bold mt-0.5 uppercase tracking-wide min-w-[70px]">Action</span>
      <p class="text-neutral-400 leading-normal">{project.solution}</p>
    </div>

    <!-- Outcomes Row -->
    <div class="flex items-start gap-3 border-t border-white/5 pt-2">
      <span class="text-brand-cyan font-mono text-[10px] font-bold mt-0.5 uppercase tracking-wide min-w-[70px]">Outcomes</span>
      <ul class="space-y-1 text-neutral-300 leading-normal w-full">
        {#each project.impact.slice(0, 2) as impactItem}
          <li class="flex items-start gap-2">
            <span class="text-brand-cyan select-none">✓</span>
            <span>{impactItem}</span>
          </li>
        {/each}
      </ul>
    </div>

  </div>

  <!-- Tech tags -->
  <div class="mt-4 flex flex-wrap gap-1.5">
    {#each project.techStack as tech}
      <span class="px-2 py-0.5 rounded text-[9px] font-mono border border-white/5 bg-neutral-900/40 text-neutral-400">
        {tech}
      </span>
    {/each}
  </div>

</div>
