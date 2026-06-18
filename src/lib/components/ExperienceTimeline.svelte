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
      date: 'June 2023 - Present',
      highlights: ['EMR Healthcare Tech', 'BPJS Integrations', 'LIS & RIS Interoperability'],
      outcomes: [
        'Developed and maintained a production Electronic Medical Record (EMR) system used by healthcare providers using Phalcon and Vue.js.',
        'Designed and implemented new functionalities, troubleshot production issues, and improved system performance and stability.',
        'Built and maintained integrations with external healthcare systems, including BPJS, Radiology Information Systems (RIS) and Laboratory Information Systems (LIS), to support healthcare interoperability.',
        'Participated in Agile Scrum ceremonies and collaborated with cross-functional teams to ensure timely feature delivery.'
      ]
    },
    {
      id: 'ginara',
      company: 'PT. Ginara Solusi Teknologi Nusantara',
      role: 'Self Employed Developer',
      location: 'Sukabumi, Indonesia',
      date: 'October 2024 - Present',
      highlights: ['Flutter Game Dev', 'Flame Engine', 'Client Web Platforms'],
      outcomes: [
        'Developed and published mobile games using Flutter and Flame Engine for the Google Play Store.',
        'Delivered responsive websites, including company profiles and interactive wedding invitation platforms, tailored to client requirements.'
      ]
    },
    {
      id: 'japi',
      company: 'Japi AI',
      role: 'Backend Developer (Freelance)',
      location: 'Sukabumi, Indonesia',
      date: 'August 2025 - October 2025',
      highlights: ['NestJS Clean Arch', 'WhatsApp Integration', 'OpenAI API Sync'],
      outcomes: [
        'Designed and developed RESTful APIs using NestJS following Clean Architecture principles to support scalable and maintainable applications.',
        'Integrated WhatsApp (whatsapp-web.js) to automate student progress notifications and improve communication with parents.',
        'Implemented OpenAI API integration to generate personalized progress summaries and actionable insights from student performance data.'
      ]
    },
    {
      id: 'mitrasinerji',
      company: 'PT. Mitra Sinerji Teknoindo',
      role: 'Web Developer',
      location: 'Bandung, Indonesia',
      date: 'July 2022 - November 2022',
      highlights: ['TypeScript & Nuxt.js', 'GraphQL & Go APIs', 'PostgreSQL'],
      outcomes: [
        'Developed responsive and interactive web application interfaces using TypeScript, Nuxt.js, and Tailwind CSS.',
        'Implemented GraphQL integrations to optimize data communication between frontend and backend services.',
        'Worked closely with cross-functional teams to transform UI/UX designs and business requirements into production-ready features.',
        'Designed and developed backend services using Golang and PostgreSQL, contributing to the delivery of end-to-end application functionality.'
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
    <h2 class="text-2xl md:text-3xl font-sans font-bold text-text-white tracking-tight">
      Work History
    </h2>
    <p class="text-text-secondary font-mono text-xs max-w-xl mt-1 leading-relaxed">
      A simple list of professional roles highlighting collaboration and direct product outcomes.
    </p>
  </div>

  <!-- Vertical Timeline -->
  <div class="relative border-l border-border-primary pl-6 ml-3 space-y-8">
    {#each experiences as exp}
      <div class="timeline-item opacity-0 translate-y-4 transition-all duration-500 ease-out relative">
        
        <!-- Circle indicator -->
        <span class="absolute -left-[31px] top-1.5 flex h-3.5 w-3.5 items-center justify-center rounded-full bg-bg-primary border border-brand-purple">
          <span class="h-1 w-1 rounded-full bg-brand-purple animate-ping"></span>
        </span>

        <!-- Compact Card -->
        <div class="glass-panel rounded-2xl p-5 hover:border-brand-purple/20 transition-all duration-300">
          
          <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center gap-1 mb-2">
            <div>
              <span class="text-brand-purple font-mono text-[10px] font-semibold uppercase tracking-wider block">{exp.date}</span>
              <h3 class="text-base font-sans font-bold text-text-white leading-tight">{exp.role}</h3>
              <p class="text-text-primary text-xs font-sans font-medium">{exp.company}</p>
            </div>
            <span class="text-text-muted font-mono text-[10px]">{exp.location}</span>
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
          <ul class="space-y-1.5 text-xs text-text-secondary font-sans leading-relaxed">
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
      <span class="absolute -left-[31px] top-5.5 flex h-3.5 w-3.5 items-center justify-center rounded-full bg-bg-primary border border-border-primary"></span>
      <div class="glass-panel rounded-2xl p-5 hover:border-brand-purple/20 transition-all duration-300">
        <h3 class="text-sm font-sans font-bold text-text-white mb-3">🎓 Education & Programs</h3>
        <div class="grid grid-cols-1 md:grid-cols-3 gap-6 text-xs font-sans">
          <div>
            <span class="text-brand-cyan font-mono text-[10px] block">2026 - Present</span>
            <span class="block text-text-white font-bold">Universitas Terbuka</span>
            <span class="text-text-secondary block text-[11px]">Information System</span>
            <span class="text-text-muted block text-[10px]">Bogor, Indonesia</span>
          </div>
          <div>
            <span class="text-brand-cyan font-mono text-[10px] block">2022 - 2023</span>
            <span class="block text-text-white font-bold">Hacktiv8 Jakarta</span>
            <span class="text-text-secondary block text-[11px]">Full Stack JavaScript (95/100)</span>
            <span class="text-text-muted block text-[10px] mb-1.5">Jakarta, Indonesia</span>
            
            <!-- Certificate Preview -->
            <a href={hacktivCertificate} target="_blank" rel="noopener noreferrer" class="block relative group overflow-hidden rounded-lg border border-border-primary w-full max-w-[140px] cursor-pointer">
              <img src={hacktivCertificate} alt="Hacktiv8 Graduate Certificate" class="w-full h-auto transition-transform duration-300 group-hover:scale-105" />
              <div class="absolute inset-0 bg-black/45 opacity-0 group-hover:opacity-100 flex items-center justify-center transition-all duration-300">
                <span class="text-[9px] font-mono text-white bg-black/80 px-2 py-0.5 rounded text-center">View Certificate ➔</span>
              </div>
            </a>
          </div>
          <div>
            <span class="text-brand-cyan font-mono text-[10px] block">2019 - 2022</span>
            <span class="block text-text-white font-bold">SMKN 2 Sukabumi</span>
            <span class="text-text-secondary block text-[11px]">Software Engineering</span>
            <span class="text-text-muted block text-[10px]">Sukabumi, Indonesia</span>
          </div>
        </div>
      </div>
    </div>

  </div>
</div>
