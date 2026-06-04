<script lang="ts">
  import { onMount } from 'svelte';
  import hacktivCertificate from '$lib/assets/hacktiv.jpg';

  interface ExperienceItem {
    id: string;
    company: string;
    role: string;
    location: string;
    date: string;
    highlights: string[];
    outcomes: string[];
  }

  const experiences: ExperienceItem[] = [
    {
      id: 'zicare',
      company: 'PT. Jejaring Tiga Artha (Zi.Care)',
      role: 'Software Engineer',
      location: 'Jakarta, Indonesia',
      date: '2023 - Present',
      highlights: ['EMR Healthcare Tech', 'BPJS Eligibility API', 'LIS Device Sync'],
      outcomes: [
        'Co-developed integrated clinical EMR APIs, implementing an automated eligibility bridge that decreased patient check-in queues by 45%.',
        'Cooperated with lab specialists to sync clinical device reports directly onto doctor charts, reducing patient result wait times by 3 hours.'
      ]
    },
    {
      id: 'ginara',
      company: 'PT. Ginara Solusi Teknologi Nusantara',
      role: 'Lead Product Developer',
      location: 'Sukabumi, Indonesia',
      date: '2024 - Present',
      highlights: ['Omnichannel SaaS', 'Golang microservice', 'SvelteKit Web'],
      outcomes: [
        'Architected ToKonek SaaS inventory worker in Go, syncing inventory levels across multiple marketplaces under 2 seconds, reducing stock discrepancies by 98%.',
        'Built bulk order administrative tools and published Flutter mobile utility applications to the Google Play Store.'
      ]
    },
    {
      id: 'japi',
      company: 'Japi AI',
      role: 'Backend Developer (Freelance)',
      location: 'Bekasi, Indonesia',
      date: '2025 (3 Mos)',
      highlights: ['NestJS Framework', 'AI Report Generator', 'WhatsApp Sync'],
      outcomes: [
        'Integrated OpenAI models to format study history into parent summaries, saving teachers up to 10 hours of manual reviews per cycle.',
        'Structured automated WhatsApp notifications mapping attendance and grades, increasing parent engagement ratings by 35%.'
      ]
    },
    {
      id: 'mitrasinerji',
      company: 'PT. Mitra Sinerji Teknoindo',
      role: 'Web Developer',
      location: 'Bandung, Indonesia',
      date: '2022 (5 Mos)',
      highlights: ['NuxtJS SSR', 'GraphQL Schemas', 'Go REST Endpoints'],
      outcomes: [
        'Coordinated with design teams to build administrative dashboards, designing GraphQL schemas that cut client-server data transfer by 40%.'
      ]
    }
  ];

  let element: HTMLElement;

  onMount(() => {
    if (!element) return;
    const items = element.querySelectorAll('.timeline-item');
    const observer = new IntersectionObserver((entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          entry.target.classList.remove('opacity-0', 'translate-y-4');
          entry.target.classList.add('opacity-100', 'translate-y-0');
          observer.unobserve(entry.target);
        }
      });
    }, { threshold: 0.1 });

    items.forEach(item => observer.observe(item));
    return () => observer.disconnect();
  });
</script>

<div bind:this={element} class="w-full max-w-5xl mx-auto px-4 py-12" id="experience">
  
  <!-- Section Title -->
  <div class="text-center md:text-left mb-10">
    <div class="inline-flex items-center gap-2 px-3 py-1 rounded-full border border-brand-purple/20 bg-brand-purple/5 mb-3">
      <span class="w-2 h-2 rounded-full bg-brand-purple animate-pulse"></span>
      <span class="text-xs font-mono text-brand-purple uppercase tracking-widest font-semibold">Timeline</span>
    </div>
    <h2 class="text-2xl md:text-3xl font-sans font-bold text-white tracking-tight">
      Work History
    </h2>
    <p class="text-neutral-400 font-mono text-xs max-w-xl mt-1 leading-relaxed">
      A simple list of professional roles highlighting collaboration and direct product outcomes.
    </p>
  </div>

  <!-- Vertical Timeline -->
  <div class="relative border-l border-white/10 pl-6 ml-3 space-y-8">
    {#each experiences as exp}
      <div class="timeline-item opacity-0 translate-y-4 transition-all duration-500 ease-out relative">
        
        <!-- Circle indicator -->
        <span class="absolute -left-[31px] top-1.5 flex h-3.5 w-3.5 items-center justify-center rounded-full bg-neutral-950 border border-brand-purple">
          <span class="h-1 w-1 rounded-full bg-brand-purple animate-ping"></span>
        </span>

        <!-- Compact Card -->
        <div class="glass-panel rounded-2xl p-5 hover:border-brand-purple/20 transition-all duration-300">
          
          <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center gap-1 mb-2">
            <div>
              <span class="text-brand-purple font-mono text-[10px] font-semibold uppercase tracking-wider block">{exp.date}</span>
              <h3 class="text-base font-sans font-bold text-white leading-tight">{exp.role}</h3>
              <p class="text-neutral-300 text-xs font-sans font-medium">{exp.company}</p>
            </div>
            <span class="text-neutral-500 font-mono text-[10px]">{exp.location}</span>
          </div>

          <!-- Tags -->
          <div class="flex flex-wrap gap-1.5 mb-3">
            {#each exp.highlights as hl}
              <span class="px-2 py-0.5 rounded text-[9px] font-mono border border-brand-purple/10 bg-brand-purple/5 text-brand-purple">
                {hl}
              </span>
            {/each}
          </div>

          <!-- Outcomes Bullet List -->
          <ul class="space-y-1.5 text-xs text-neutral-400 font-sans leading-relaxed">
            {#each exp.outcomes as outcome}
              <li class="flex items-start gap-2">
                <span class="text-brand-purple select-none">•</span>
                <span>{outcome}</span>
              </li>
            {/each}
          </ul>

        </div>
      </div>
    {/each}

    <!-- Education Entry (Combined & Small) -->
    <div class="timeline-item opacity-0 translate-y-4 transition-all duration-500 ease-out relative pt-4">
      <span class="absolute -left-[31px] top-5.5 flex h-3.5 w-3.5 items-center justify-center rounded-full bg-neutral-950 border border-neutral-700"></span>
      <div class="glass-panel rounded-2xl p-5 border border-white/5 hover:border-neutral-700">
        <h3 class="text-sm font-sans font-bold text-white mb-3">🎓 Education & Programs</h3>
        <div class="grid grid-cols-1 sm:grid-cols-2 gap-4 text-xs font-sans">
          <div>
            <span class="text-brand-cyan font-mono text-[10px] block">2022 - 2023</span>
            <span class="block text-white font-bold">Hacktiv8 Jakarta</span>
            <span class="text-neutral-400 block text-[11px]">Full Stack JS (Grade 95/100)</span>
            
            <!-- Certificate Preview -->
            <a href={hacktivCertificate} target="_blank" rel="noopener noreferrer" class="block mt-2.5 relative group overflow-hidden rounded-lg border border-white/10 w-full max-w-[150px] cursor-pointer">
              <img src={hacktivCertificate} alt="Hacktiv8 Graduate Certificate" class="w-full h-auto transition-transform duration-300 group-hover:scale-105" />
              <div class="absolute inset-0 bg-black/45 opacity-0 group-hover:opacity-100 flex items-center justify-center transition-all duration-300">
                <span class="text-[9px] font-mono text-white bg-black/80 px-2 py-0.5 rounded">View Certificate ➔</span>
              </div>
            </a>
          </div>
          <div>
            <span class="text-brand-cyan font-mono text-[10px] block">2019 - 2022</span>
            <span class="block text-white font-bold">SMKN 2 Sukabumi</span>
            <span class="text-neutral-400 block text-[11px]">Software Engineering Major</span>
          </div>
        </div>
      </div>
    </div>

  </div>
</div>
