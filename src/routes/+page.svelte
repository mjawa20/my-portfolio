<script lang="ts">
  import { onMount } from 'svelte';
  import MetricCounters from '$lib/components/MetricCounters.svelte';
  import ProjectCard, { type Project } from '$lib/components/ProjectCard.svelte';
  import TechOrbit from '$lib/components/TechOrbit.svelte';
  import ExperienceTimeline from '$lib/components/ExperienceTimeline.svelte';
  import Testimonials from '$lib/components/Testimonials.svelte';

  // Hero interactive typing headlines
  const headlineTexts = [
    'Designing software systems that solve business challenges.',
    'Building applications that medical teams and merchants trust.',
    'Integrating secure services to improve patient check-ins.',
    'Collaborating with product teams to build fast SaaS platforms.'
  ];
  let currentTextIndex = $state(0);
  let displayedText = $state('');

  // CV Download state & Toast notification
  let toastMessage = $state('');
  let showToast = $state(false);

  function triggerToast(msg: string) {
    toastMessage = msg;
    showToast = true;
    setTimeout(() => {
      showToast = false;
    }, 3000);
  }

  function downloadCV() {
    triggerToast('🚀 Downloading CV_Muhammad_Jawahiruzzaman.pdf');
    const link = document.createElement('a');
    link.href = '#';
    link.download = 'CV_Muhammad_Jawahiruzzaman.pdf';
    
    const placeholderContent = `Muhammad Jawahiruzzaman - Software Engineer\n\nContact:\n+6285728888280\nm.jawahiruzzaman@gmail.com\n\nSummary:\nSoftware Engineer experienced in EMR platforms and SaaS marketplace omnichannel dashboards. Focused on clean code, collaboration, and business outcomes.`;
    const blob = new Blob([placeholderContent], { type: 'text/plain' });
    link.href = URL.createObjectURL(blob);
    
    setTimeout(() => {
      document.body.appendChild(link);
      link.click();
      document.body.removeChild(link);
      triggerToast('✔ Download complete.');
    }, 1000);
  }

  // Typewriter Loop
  onMount(() => {
    let textIdx = 0;
    let erase = false;
    let timer: any;

    function runType() {
      const fullText = headlineTexts[currentTextIndex];

      if (!erase) {
        displayedText = fullText.slice(0, textIdx + 1);
        textIdx++;
        if (textIdx === fullText.length) {
          erase = true;
          timer = setTimeout(runType, 2200);
        } else {
          timer = setTimeout(runType, 35);
        }
      } else {
        displayedText = fullText.slice(0, textIdx - 1);
        textIdx--;
        if (textIdx === 0) {
          erase = false;
          currentTextIndex = (currentTextIndex + 1) % headlineTexts.length;
          timer = setTimeout(runType, 2500); // Hold blank state briefly
        } else {
          timer = setTimeout(runType, 15);
        }
      }
    }

    runType();
    return () => clearTimeout(timer);
  });

  // Projects data
  const projects: Project[] = [
    {
      id: 'emr',
      title: 'Electronic Medical Record System',
      tagline: 'Patient care application utilized in medical clinics to secure patient charts and speed up clinic check-in operations.',
      category: 'Healthcare Technology',
      role: 'Software Engineer',
      themeColor: 'cyan',
      problem: 'Clinic staff spent hours validating patient insurance and entering laboratory devices logs manually, creating check-in delays.',
      solution: 'Developed integrated EMR services. Connected automated insurance check systems (BPJS) and Laboratory reports (LIS) to synchronize patient logs instantly.',
      techStack: ['PHP', 'Phalcon', 'Vue.js', 'PostgreSQL', 'MySQL', 'Redis', 'BPJS API', 'LIS (HL7)'],
      impact: [
        'Reduced patient clinic check-in wait times by 45% using secure API bridges.',
        'Automated LIS lab results sync, enabling doctors to review files instantly and saving hours of administrative transcription.'
      ]
    },
    {
      id: 'tokonek',
      title: 'ToKonek SaaS Omnichannel',
      tagline: 'Multi-channel business application syncing inventory and order status in real-time to protect merchant ratings.',
      category: 'SaaS Omnichannel',
      role: 'Lead Developer',
      themeColor: 'purple',
      problem: 'SaaS merchants waste hours updating catalog stocks manually across online stores (Shopee, Tokopedia, etc.), creating double-sold inventory issues and shipment delays.',
      solution: 'Built the ToKonek stock sync engine, managing background update databases and database queries in Go to sync inventory quantities across channels under 2 seconds.',
      techStack: ['Golang', 'SvelteKit', 'TypeScript', 'Tailwind CSS', 'PostgreSQL', 'Gorm', 'Redis', 'MinIo'],
      impact: [
        'Synchronized catalog stocks across online channels under 2 seconds, reducing stock discrepancies by 98%.',
        'Engineered bulk order document tools generating invoices with zero performance delays.'
      ]
    },
    {
      id: 'esertifikasi',
      title: 'E-Sertifikasi NextGen (Kominfo)',
      tagline: 'Official administrative certification portal from Kominfo for secure digital audits and document approvals.',
      category: 'Government Project',
      role: 'Web Developer',
      themeColor: 'emerald',
      problem: 'Manual document routing, physical audits, and physical approval cycles created multi-week backlogs for regional certification sign-offs.',
      solution: 'Developed a secure government portal, implementing automated verification status loops to review and check off certifications, decreasing administrative file backlogs.',
      techStack: ['Golang', 'Nuxt.js', 'Vue.js', 'TypeScript', 'Tailwind CSS', 'PostgreSQL', 'GraphQL', 'Gorm'],
      impact: [
        'Accelerated document review-to-issuance times from 14 days down to 6 hours.',
        'Reduced client network payload transfer sizes by 40% using custom data resolvers.'
      ]
    }
  ];

  // Contact Form State
  let contactName = $state('');
  let contactEmail = $state('');
  let contactMessage = $state('');
  let isSending = $state(false);
  let formSent = $state(false);

  function handleContactSubmit(e: SubmitEvent) {
    e.preventDefault();
    if (!contactName || !contactEmail || !contactMessage) return;

    isSending = true;
    setTimeout(() => {
      isSending = false;
      formSent = true;
      contactName = '';
      contactEmail = '';
      contactMessage = '';
      triggerToast('✔ Message sent successfully!');
    }, 1200);
  }
</script>

<!-- Toast System Logs -->
{#if showToast}
  <div class="fixed bottom-6 right-6 z-50 glass-panel rounded-lg p-4 font-mono text-xs border border-brand-cyan bg-black/90 shadow-[0_0_20px_rgba(6,182,212,0.2)] max-w-sm text-emerald-400">
    <div class="flex items-center gap-2">
      <span class="w-2 h-2 rounded-full bg-emerald-500 animate-pulse"></span>
      <span>{toastMessage}</span>
    </div>
  </div>
{/if}

<div class="w-full flex flex-col items-center">
  
  <!-- HERO & BIO INTEGRATED CONSOLE SECTION -->
  <section id="hero" class="w-full max-w-5xl mx-auto px-4 pt-12 pb-6">
    <div class="glass-panel rounded-3xl p-6 md:p-10 relative overflow-hidden">
      <!-- Glow decoration -->
      <div class="absolute -right-32 -bottom-32 w-64 h-64 rounded-full bg-brand-purple/5 blur-3xl pointer-events-none"></div>

      <!-- Status indicator -->
      <div class="inline-flex items-center gap-2 px-3 py-1 rounded-full border border-white/5 bg-neutral-900/50 backdrop-blur-md mb-6">
        <span class="relative flex h-2 w-2">
          <span class="animate-ping absolute inline-flex h-full w-full rounded-full bg-brand-cyan opacity-75"></span>
          <span class="relative inline-flex rounded-full h-2 w-2 bg-brand-cyan"></span>
        </span>
        <span class="text-[10px] font-mono text-neutral-300 font-medium uppercase tracking-wider">
          Active // Available for Full Stack & Backend Roles
        </span>
      </div>

      <!-- Bio Layout -->
      <div class="grid grid-cols-1 lg:grid-cols-12 gap-8 items-center border-b border-white/5 pb-8">
        
        <!-- Left: Profile Initials Avatar Card -->
        <div class="lg:col-span-4 flex flex-col items-center justify-center text-center">
          <div class="w-28 h-28 md:w-36 md:h-36 rounded-full bg-gradient-to-tr from-brand-cyan via-brand-purple to-brand-emerald p-[2px] shadow-[0_0_25px_rgba(139,92,246,0.15)] mb-3">
            <div class="w-full h-full bg-neutral-950 rounded-full flex items-center justify-center text-white font-mono text-2xl md:text-3xl font-extrabold">
              MJ
            </div>
          </div>
          <span class="text-white font-bold text-base font-sans">Muhammad Jawahiruzzaman</span>
          <span class="text-neutral-500 font-mono text-[10px] mt-0.5">Based in Sukabumi, Indonesia</span>
        </div>

        <!-- Right: Bio copy & CTAs -->
        <div class="lg:col-span-8 space-y-4">
          <h1 class="text-3xl sm:text-5xl font-sans font-extrabold tracking-tight text-white">
            Muhammad Jawahiruzzaman
          </h1>
          
          <div class="min-h-[3rem] sm:min-h-[2.5rem] flex items-center">
            <p class="text-sm sm:text-base text-brand-cyan font-mono leading-relaxed border-r border-brand-cyan pr-2 animate-caret">
              {displayedText}
            </p>
          </div>

          <p class="text-neutral-300 text-xs sm:text-sm leading-relaxed font-sans font-normal max-w-2xl">
            Software Engineer with experience in healthcare Electronic Medical Records (EMR) and SaaS marketplace omnichannel platforms. I design software with the end-user in mind, focus on business outcomes, and maintain transparent, friendly collaboration with product teams.
          </p>

          <!-- Core CTAs -->
          <div class="flex flex-wrap items-center gap-3 pt-2">
            <a href="#projects" class="px-5 py-2 rounded-full bg-white text-black font-semibold font-sans text-xs hover:bg-neutral-200 transition-all btn-magnetic cursor-pointer">
              View Projects
            </a>
            
            <button onclick={downloadCV} class="px-5 py-2 rounded-full border border-white/10 bg-neutral-900/60 text-white font-semibold font-sans text-xs hover:bg-neutral-800 transition-all btn-magnetic cursor-pointer flex items-center gap-1.5">
              <svg class="w-3.5 h-3.5 text-brand-cyan" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-4l-4 4m0 0l-4-4m4 4V4"/></svg>
              Resume PDF
            </button>

            <a href="#contact" class="px-5 py-2 rounded-full border border-white/5 bg-white/5 text-neutral-300 font-semibold font-sans text-xs hover:bg-white/10 transition-all btn-magnetic cursor-pointer">
              Contact
            </a>
          </div>
        </div>

      </div>

      <!-- Compact Values Row (Sitting right inside hero panel) -->
      <div class="grid grid-cols-1 sm:grid-cols-3 gap-4 pt-6 text-xs">
        <div class="space-y-1">
          <h4 class="text-brand-cyan font-bold font-sans flex items-center gap-1">
            <span>✔</span> User-Focus
          </h4>
          <p class="text-neutral-500 leading-normal font-sans">
            Designing code that saves clinic doctors and marketplace merchants valuable daily hours.
          </p>
        </div>
        
        <div class="space-y-1">
          <h4 class="text-brand-purple font-bold font-sans flex items-center gap-1">
            <span>🤝</span> Team Collaboration
          </h4>
          <p class="text-neutral-500 leading-normal font-sans">
            Maintaining direct, friendly alignment with product managers, designers, and web leads.
          </p>
        </div>

        <div class="space-y-1">
          <h4 class="text-brand-emerald font-bold font-sans flex items-center gap-1">
            <span>⛁</span> Clean Quality
          </h4>
          <p class="text-neutral-500 leading-normal font-sans">
            Writing tested, containerised software running smoothly in production under active user loads.
          </p>
        </div>
      </div>

    </div>
  </section>

  <!-- METRICS SECTION (Compact & Subtle) -->
  <section class="w-full max-w-5xl mx-auto py-2">
    <MetricCounters />
  </section>

  <!-- FEATURED PROJECTS SECTION -->
  <section id="projects" class="w-full max-w-5xl mx-auto px-4 py-8">
    <!-- Section Title -->
    <div class="text-center md:text-left mb-8">
      <div class="inline-flex items-center gap-2 px-3 py-1 rounded-full border border-brand-cyan/20 bg-brand-cyan/5 mb-3">
        <span class="w-2 h-2 rounded-full bg-brand-cyan animate-pulse"></span>
        <span class="text-xs font-mono text-brand-cyan uppercase tracking-widest font-semibold">Showcases</span>
      </div>
      <h2 class="text-2xl md:text-3xl font-sans font-bold text-white tracking-tight">
        Featured Projects
      </h2>
      <p class="text-neutral-400 font-mono text-xs max-w-xl mt-1 leading-relaxed">
        Real-world applications delivering measurable business outcomes and operational value.
      </p>
    </div>

    <!-- Projects Grid (Compact card view) -->
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
      {#each projects as proj}
        <ProjectCard project={proj} />
      {/each}
    </div>
  </section>

  <!-- TECH STACK SECTION (Compact Grid Cloud) -->
  <section class="w-full max-w-5xl mx-auto py-4">
    <TechOrbit />
  </section>

  <!-- EXPERIENCE TIMELINE SECTION (Compact timeline) -->
  <section class="w-full max-w-5xl mx-auto py-4">
    <ExperienceTimeline />
  </section>

  <!-- TESTIMONIALS SECTION (Micro view) -->
  <section class="w-full max-w-5xl mx-auto py-4">
    <Testimonials />
  </section>

  <!-- CONTACT SECTION (Compact Grid Card) -->
  <section id="contact" class="w-full max-w-5xl mx-auto px-4 py-12">
    <div class="glass-panel relative rounded-3xl p-6 md:p-8 overflow-hidden hover:border-brand-cyan/20 transition-all duration-300">
      
      <!-- Top glow line -->
      <div class="absolute top-0 left-0 right-0 h-[2px] bg-gradient-to-r from-transparent via-brand-cyan to-transparent"></div>
      
      <!-- Content Grid -->
      <div class="grid grid-cols-1 md:grid-cols-12 gap-8 items-center">
        
        <!-- Left: Quick Direct Contact Details -->
        <div class="md:col-span-5 space-y-4">
          <div class="inline-flex items-center gap-2 px-3 py-1 rounded-full border border-brand-cyan/20 bg-brand-cyan/5 mb-1">
            <span class="w-2 h-2 rounded-full bg-brand-cyan animate-pulse"></span>
            <span class="text-[10px] font-mono text-brand-cyan uppercase tracking-widest font-semibold">Connect</span>
          </div>
          
          <h2 class="text-2xl md:text-3xl font-sans font-bold text-white tracking-tight leading-tight">
            Let's build together.
          </h2>
          
          <p class="text-neutral-400 text-xs leading-relaxed font-sans">
            Interested in scaling systems or have an open role on your developer team? Let's discuss directly.
          </p>

          <div class="space-y-3 pt-3 border-t border-white/5">
            <a href="mailto:m.jawahiruzzaman@gmail.com" class="flex items-center gap-2.5 text-neutral-300 hover:text-brand-cyan transition-colors text-xs font-mono group cursor-pointer">
              <span class="w-7 h-7 rounded-lg bg-white/5 border border-white/10 flex items-center justify-center font-bold text-neutral-400 group-hover:text-brand-cyan">@</span>
              <span>m.jawahiruzzaman@gmail.com</span>
            </a>
            
            <a href="tel:+6285728888280" class="flex items-center gap-2.5 text-neutral-300 hover:text-brand-cyan transition-colors text-xs font-mono group cursor-pointer">
              <span class="w-7 h-7 rounded-lg bg-white/5 border border-white/10 flex items-center justify-center font-bold text-neutral-400 group-hover:text-brand-cyan">☏</span>
              <span>+6285728888280</span>
            </a>

            <!-- Social Links Row -->
            <div class="flex gap-3 pt-1">
              <a href="https://github.com" target="_blank" rel="noopener noreferrer" class="px-3 py-1.5 rounded-lg border border-white/5 bg-neutral-950/60 font-mono text-[9px] text-neutral-400 hover:text-white hover:border-white/20 transition-all cursor-pointer">
                GITHUB
              </a>
              <a href="https://linkedin.com" target="_blank" rel="noopener noreferrer" class="px-3 py-1.5 rounded-lg border border-white/5 bg-neutral-950/60 font-mono text-[9px] text-neutral-400 hover:text-white hover:border-white/20 transition-all cursor-pointer">
                LINKEDIN
              </a>
            </div>
          </div>
        </div>

        <!-- Right: Simple Compact Message Sender -->
        <div class="md:col-span-7">
          <form onsubmit={handleContactSubmit} class="glass-panel rounded-2xl p-5 space-y-3 font-sans text-xs">
            <div class="grid grid-cols-1 sm:grid-cols-2 gap-3">
              <div>
                <label for="name" class="block text-neutral-400 font-mono text-[10px] uppercase mb-1">Your Name</label>
                <input id="name" type="text" bind:value={contactName} required placeholder="Full Name" class="w-full bg-neutral-900 border border-white/5 rounded-xl px-3 py-2 text-white placeholder-neutral-600 focus:border-brand-cyan focus:outline-none transition-all" />
              </div>
              <div>
                <label for="email" class="block text-neutral-400 font-mono text-[10px] uppercase mb-1">Your Email</label>
                <input id="email" type="email" bind:value={contactEmail} required placeholder="email@domain.com" class="w-full bg-neutral-900 border border-white/5 rounded-xl px-3 py-2 text-white placeholder-neutral-600 focus:border-brand-cyan focus:outline-none transition-all" />
              </div>
            </div>
            <div>
              <label for="message" class="block text-neutral-400 font-mono text-[10px] uppercase mb-1">Your Message</label>
              <textarea id="message" bind:value={contactMessage} required rows="3" placeholder="Compose message..." class="w-full bg-neutral-900 border border-white/5 rounded-xl px-3 py-2 text-white placeholder-neutral-600 focus:border-brand-cyan focus:outline-none transition-all resize-none"></textarea>
            </div>
            
            <button type="submit" disabled={isSending} class="w-full py-2.5 rounded-xl border border-brand-cyan bg-brand-cyan/10 text-brand-cyan font-bold cursor-pointer hover:bg-brand-cyan/20 transition-all duration-300 font-mono text-xs text-center">
              {#if isSending}SENDING_MESSAGE...{:else if formSent}MESSAGE_DISPATCHED ✓{:else}SEND CONNECTION MESSAGE(){/if}
            </button>
          </form>
        </div>

      </div>
    </div>
  </section>

</div>

<style>
  /* Typewriter blinking cursor */
  @keyframes blink {
    50% { border-color: transparent; }
  }
  .animate-caret {
    animation: blink 0.75s step-end infinite;
  }
</style>
