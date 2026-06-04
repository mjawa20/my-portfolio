<script lang="ts">
  interface TechCategory {
    title: string;
    description: string;
    skills: string[];
    themeClass: string;
    glowClass: string;
  }

  const categories: TechCategory[] = [
    {
      title: "Core Backend & APIs",
      description: "Fast backend logic and services.",
      skills: ["Golang", "Phalcon (PHP)", "NestJS", "Laravel", "Node.js", "Gorm", "REST API"],
      themeClass: "border-brand-cyan/20 bg-brand-cyan/5 text-brand-cyan",
      glowClass: "group-hover:border-brand-cyan/35 hover:shadow-[0_0_15px_rgba(6,182,212,0.15)] hover:text-brand-cyan hover:border-brand-cyan/40"
    },
    {
      title: "Frontend & Web client",
      description: "Modern, fast-loading responsive UIs.",
      skills: ["SvelteKit", "Vue.js", "TypeScript", "Nuxt.js", "Tailwind CSS", "ReactJS"],
      themeClass: "border-brand-purple/20 bg-brand-purple/5 text-brand-purple",
      glowClass: "group-hover:border-brand-purple/35 hover:shadow-[0_0_15px_rgba(139,92,246,0.15)] hover:text-brand-purple hover:border-brand-purple/40"
    },
    {
      title: "Databases & Cache",
      description: "Safe transaction writes and cache registers.",
      skills: ["PostgreSQL", "Redis", "MySQL", "Firebase", "MongoDB"],
      themeClass: "border-brand-emerald/20 bg-brand-emerald/5 text-brand-emerald",
      glowClass: "group-hover:border-brand-emerald/35 hover:shadow-[0_0_15px_rgba(16,185,129,0.15)] hover:text-brand-emerald hover:border-brand-emerald/40"
    },
    {
      title: "DevOps & Integrations",
      description: "Uniform container deploy and APIs bridging.",
      skills: ["Docker", "BPJS Integration", "LIS Integration", "GraphQL", "MinIo"],
      themeClass: "border-neutral-700/30 bg-neutral-900/50 text-neutral-300",
      glowClass: "group-hover:border-neutral-500/40 hover:shadow-[0_0_15px_rgba(255,255,255,0.08)] hover:text-white hover:border-white/30"
    }
  ];
</script>

<div class="w-full max-w-7xl mx-auto px-4 py-12" id="skills">
  
  <!-- Section Title -->
  <div class="text-center md:text-left mb-10">
    <div class="inline-flex items-center gap-2 px-3 py-1 rounded-full border border-brand-cyan/20 bg-brand-cyan/5 mb-3">
      <span class="w-2 h-2 rounded-full bg-brand-cyan animate-pulse"></span>
      <span class="text-xs font-mono text-brand-cyan uppercase tracking-widest font-semibold">Capabilities</span>
    </div>
    <h2 class="text-2xl md:text-3xl font-sans font-bold text-white tracking-tight">
      Technology Stack
    </h2>
    <p class="text-neutral-400 font-mono text-xs max-w-xl mt-1 leading-relaxed">
      A compact summary of core languages, database systems, and integrations utilized to build production software.
    </p>
  </div>

  <!-- Tech Grid Cloud -->
  <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4">
    {#each categories as cat}
      <div class="glass-panel rounded-2xl p-5 hover:border-white/10 transition-all duration-300 group flex flex-col justify-between">
        <div>
          <!-- Header -->
          <div class="flex items-center justify-between mb-2">
            <h3 class="text-sm font-sans font-bold text-white tracking-tight">{cat.title}</h3>
            <span class="text-[9px] font-mono px-2 py-0.5 rounded-full border {cat.themeClass}">Active</span>
          </div>
          
          <p class="text-neutral-500 text-[11px] font-sans mb-4">{cat.description}</p>
          
          <!-- Skill Tags -->
          <div class="flex flex-wrap gap-2">
            {#each cat.skills as skill}
              <span class="px-2.5 py-1 rounded-lg text-xs font-mono border border-white/5 bg-neutral-900/60 text-neutral-400 transition-all duration-300 cursor-pointer {cat.glowClass}">
                {skill}
              </span>
            {/each}
          </div>
        </div>
      </div>
    {/each}
  </div>

</div>
