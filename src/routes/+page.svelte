<script lang="ts">
  import { onMount } from 'svelte';
  import ProjectCard, { type Project } from '$lib/components/ProjectCard.svelte';
  import TechOrbit from '$lib/components/TechOrbit.svelte';
  import ExperienceTimeline from '$lib/components/ExperienceTimeline.svelte';
  import profilePhoto from '$lib/assets/profile.jpg';

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
    
    const placeholderContent = `Muhammad Jawahiruzzaman - Software Engineer

Jakarta, Indonesia | +6285728888280 | m.jawahiruzzaman@gmail.com | LinkedIn | GitHub

SUMMARY
Software Engineer with 3+ years of experience building production-grade applications across healthcare and business domains. Skilled in backend and full-stack development using PHP, Go, JavaScript, and TypeScript, with hands-on experience in Phalcon, Vue.js, SvelteKit, and NestJS. Experienced in integrating healthcare systems, including BPJS, Laboratory Information Systems (LIS), and Radiology Information Systems (RIS), to support interoperability and streamline operations. Proven ability to deliver reliable, maintainable, and user-focused solutions while collaborating effectively within Agile teams.

EDUCATION
- Universitas Terbuka Bogor, Indonesia: Information System (2026 - Present)
- Hacktiv8 Jakarta, Indonesia: Full Stack JavaScript Immersive Program (2022 - 2023) - Grade 95/100
- SMKN 2 Sukabumi Sukabumi, Indonesia: Software Engineering (2019 - 2022)

WORK EXPERIENCE
- PT. Jejaring Tiga Artha (Zi.Care) (June 2023 - Present) - Software Engineer
  * Developed and maintained a production Electronic Medical Record (EMR) system using Phalcon and Vue.js.
  * Designed and implemented new functionalities, troubleshot production issues, and improved performance and stability.
  * Participated in Agile Scrum ceremonies and collaborated with cross-functional teams.
  * Integrated external healthcare systems (BPJS, RIS, LIS) for healthcare interoperability.
- PT. Ginara Solusi Teknologi Nusantara (October 2024 - Present) - Self Employed
  * Developed and published mobile games using Flutter and Flame Engine for the Google Play Store.
  * Delivered responsive websites, company profiles, and interactive wedding invitations.
- Japi AI (August 2025 - October 2025) - Backend Freelance
  * Designed and developed RESTful APIs using NestJS following Clean Architecture principles.
  * Integrated WhatsApp (whatsapp-web.js) to automate student progress notifications.
  * Implemented OpenAI API integration to generate personalized student performance summaries.
- PT. Mitra Sinerji Teknoindo (July 2022 - November 2022) - Web Developer
  * Developed responsive web interfaces using TypeScript, Nuxt.js, and Tailwind CSS.
  * Implemented GraphQL integrations to optimize data communication.
  * Worked closely with cross-functional teams to translate UI/UX designs into production features.
  * Designed and developed backend services using Golang and PostgreSQL.

SKILLS
- Languages: PHP, JavaScript, TypeScript, Go, Dart
- Front End: Vue.js, SvelteKit, Nuxt.js, React.js, Flutter, Tailwind CSS, Pinia, Redux, HTML, CSS
- Back End: Phalcon, NestJS, Express.js, Gin, Laravel, REST API, GraphQL, RabbitMQ
- Databases & Storage: PostgreSQL, MariaDB, MongoDB, Redis, MinIO
- Tools & DevOps: Docker, Git, Firebase

PROJECTS
- WordRush: Charades Party Game (March 2026) - Flutter + Flame Engine, Firebase (Firestore)
- Tokonek (June 2025) - TypeScript, SvelteJS, TailwindCSS, Go, PostgreSQL, MinIO, Gorm
- E-Sertifikasi NextGen (October 2022) - VueJS, NuxtJS, TailwindCSS, Go, PostgreSQL, GraphQL, Gorm, TypeScript

CERTIFICATIONS
- HackerRank: JavaScript (Intermediate) - March 2023
- HackerRank: Problem Solving (Intermediate) - March 2021`;
    const blob = new Blob([placeholderContent], { type: 'text/plain' });
    link.href = URL.createObjectURL(blob);
    
    setTimeout(() => {
      document.body.appendChild(link);
      link.click();
      document.body.removeChild(link);
      triggerToast('✔ Download complete.');
    }, 1000);
  }

  // Typewriter Loop & Intersection Observer for Scroll Reveal
  onMount(() => {
    // Scroll reveal observer
    const observerOptions = {
      root: null,
      rootMargin: '0px',
      threshold: 0.1
    };

    const observer = new IntersectionObserver((entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          entry.target.classList.add('revealed');
          observer.unobserve(entry.target);
        }
      });
    }, observerOptions);

    const revealItems = document.querySelectorAll('.reveal-item');
    revealItems.forEach((item) => observer.observe(item));

    // Typewriter loop
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
    return () => {
      clearTimeout(timer);
      observer.disconnect();
    };
  });

  // Projects data
  const projects: Project[] = [
    {
      id: 'wordrush',
      title: 'WordRush: Charades Party Game',
      tagline: 'Multiplayer charades party game published on Google Play Store with custom decks and real-time gameplay.',
      category: 'Mobile Game',
      role: 'Self Employed Developer',
      themeColor: 'purple',
      problem: 'Party games often lack customizable decks and localization options, leading to repetitive gameplay.',
      solution: 'Designed and developed a multiplayer game using Flutter and Flame Engine, integrating Firebase Firestore for real-time deck synchronization and custom deck creation.',
      techStack: ['Flutter', 'Flame Engine', 'Dart', 'Firebase (Firestore)', 'Google Play Console'],
      impact: [
        'Published a fully featured game to the Google Play Store with custom theme options.',
        'Implemented real-time local/party state and custom deck management for enhanced replayability.'
      ],
      link: 'https://play.google.com/store/apps/details?id=com.eamonstudio.word_rush',
      github: 'https://github.com/mjawa20/word-rush',
      images: ['/project_wordrush_mockup.png']
    },
    {
      id: 'emr',
      title: 'Electronic Medical Record (EMR)',
      tagline: 'Production-grade healthcare platform supporting clinical workflows and hospital interoperability.',
      category: 'Healthcare Technology',
      role: 'Software Engineer',
      themeColor: 'cyan',
      problem: 'Healthcare providers struggle with manual check-ins, delayed lab report synchronization, and disjointed insurance validation.',
      solution: 'Built and integrated EMR services using Phalcon and Vue.js, connecting external systems like BPJS, Radiology (RIS), and Laboratory (LIS).',
      techStack: ['PHP', 'Phalcon', 'Vue.js', 'PostgreSQL', 'Redis', 'BPJS API', 'LIS (HL7)', 'RIS Integration'],
      impact: [
        'Streamlined hospital workflows and enhanced patient check-in speeds through automated systems.',
        'Enabled real-time diagnostic reporting by integrating laboratory and radiology information systems.'
      ],
      link: 'https://zicare.id/',
      images: ['/project_emr_mockup.png']
    },
    {
      id: 'tokonek',
      title: 'Tokonek Business Solutions',
      tagline: 'Business and marketplace management solutions for omnichannel inventory syncing.',
      category: 'SaaS Omnichannel',
      role: 'Full-Stack Developer',
      themeColor: 'cyan',
      problem: 'SaaS merchants waste hours updating catalog stocks manually across online stores, leading to stock discrepancies.',
      solution: 'Built business and marketplace management solutions using Svelte, Go, and PostgreSQL with MinIO for storage.',
      techStack: [ 'TypeScript', 'SvelteJS', 'TailwindCSS', 'PostgreSQL', 'MinIO', 'Gorm', 'Redis', 'Go'],
      impact: [
        'Developed inventory management and omnichannel solutions to synchronize business operations.',
        'Implemented robust file storage using MinIO and clean database handling with PostgreSQL and Gorm.'
      ],
      link: 'https://tokonek.id/',
      images: ['/project_tokonek_mockup.png']
    },
    {
      id: 'esertifikasi',
      title: 'E-Sertifikasi NextGen',
      tagline: 'Official certification application portal developed for Kominfo for streamlined digital document processing.',
      category: 'Government Project',
      role: 'Web Developer',
      themeColor: 'emerald',
      problem: 'Manual certification processes and physical document routing created long processing delays and high administrative overhead.',
      solution: 'Designed and implemented Nuxt.js/Vue.js frontend and Go backend API with GraphQL resolvers and PostgreSQL database.',
      techStack: ['Go', 'TypeScript', 'VueJS', 'NuxtJS', 'TailwindCSS', 'PostgreSQL', 'GraphQL', 'Gorm'],
      impact: [
        'Created responsive and interactive interfaces to optimize government administrative workflow.',
        'Leveraged GraphQL schemas to improve client-server communication and optimize network payloads.'
      ],
      link: '#',
      github: 'https://sertifikasi.postel.go.id/',
      images: ['/project_esertifikasi_mockup.png']
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
  <section id="hero" class="w-full max-w-5xl mx-auto px-4 pt-12 pb-6 reveal-item">
    <div class="glass-panel rounded-3xl p-6 md:p-10 relative overflow-hidden">
      <!-- Glow decoration -->
      <div class="absolute -right-32 -bottom-32 w-64 h-64 rounded-full bg-brand-purple/5 blur-3xl pointer-events-none"></div>

      <!-- Status indicator -->
      <!-- <div class="inline-flex items-center gap-2 px-3 py-1 rounded-full border border-border-primary bg-bg-secondary/50 backdrop-blur-md mb-6">
        <span class="relative flex h-2 w-2">
          <span class="animate-ping absolute inline-flex h-full w-full rounded-full bg-brand-cyan opacity-75"></span>
          <span class="relative inline-flex rounded-full h-2 w-2 bg-brand-cyan"></span>
        </span>
        <span class="text-[10px] font-mono text-text-secondary font-medium uppercase tracking-wider">
          Active // Available for Full Stack & Backend Roles
        </span>
      </div> -->

      <!-- Bio Layout -->
      <div class="grid grid-cols-1 lg:grid-cols-12 gap-8 items-center pb-8">
        
        <!-- Left: Profile Avatar Card -->
        <div class="lg:col-span-5 flex flex-col items-center justify-center text-center">
          <div class="relative w-36 h-36 sm:w-44 sm:h-44 md:w-52 md:h-52 lg:w-56 lg:h-56 mb-4 animate-float">
            <!-- Pulsing backing glow -->
            <div class="absolute inset-0 rounded-full bg-gradient-to-tr from-brand-cyan/20 via-brand-purple/20 to-brand-emerald/20 blur-xl opacity-85 transition-opacity animate-pulse pointer-events-none"></div>
            
            <!-- Photo Frame with custom styling filters -->
            <div 
              class="w-full h-full rounded-full bg-gradient-to-b from-bg-primary/40 to-bg-secondary/80 p-[2px] border border-border-primary relative overflow-hidden group hover:border-brand-cyan/45 transition-all duration-300"
              style:box-shadow="var(--card-shadow)"
            >
              <img 
                src={profilePhoto} 
                alt="Muhammad Jawahiruzzaman" 
                class="w-full h-full object-cover object-center transition-all duration-500 group-hover:scale-105"
              />
            </div>
          </div>
          <span class="text-text-white font-bold text-base font-sans">Muhammad Jawahiruzzaman</span>
          <span class="text-text-muted font-mono text-[10px] mt-0.5">Jakarta, Indonesia</span>
        </div>

        <!-- Right: Bio copy & CTAs -->
        <div class="lg:col-span-7 space-y-4 text-center lg:text-left">
          <h1 class="text-3xl sm:text-5xl font-sans font-extrabold tracking-tight text-text-white">
            Muhammad Jawahiruzzaman
          </h1>
          
          <div class="min-h-[3rem] sm:min-h-[2.5rem] flex items-center justify-center lg:justify-start">
            <p class="text-sm sm:text-base text-brand-cyan font-mono leading-relaxed border-r border-brand-cyan pr-2 animate-caret">
              {displayedText}
            </p>
          </div>

          <p class="text-text-primary text-xs sm:text-sm leading-relaxed font-sans font-normal max-w-2xl mx-auto lg:mx-0">
            Software Engineer with 3+ years of experience building production-grade applications across healthcare and business domains. Skilled in backend and full-stack development, mobile game creation with Flutter, and integrating complex healthcare and automation systems.
          </p>

          <!-- Core CTAs -->
          <div class="flex flex-wrap items-center justify-center lg:justify-start gap-3 pt-2">
            <a href="#projects" class="px-5 py-2 rounded-full bg-text-white text-bg-primary font-semibold font-sans text-xs hover:opacity-90 transition-all btn-magnetic cursor-pointer shadow-sm">
              View Projects
            </a>
            
            <button onclick={downloadCV} class="px-5 py-2 rounded-full border border-border-primary bg-bg-secondary/60 text-text-white hover:bg-btn-hover font-semibold font-sans text-xs transition-all btn-magnetic cursor-pointer flex items-center gap-1.5 shadow-sm">
              <svg class="w-3.5 h-3.5 text-brand-cyan" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-4l-4 4m0 0l-4-4m4 4V4"/></svg>
              Resume PDF
            </button>

            <a href="#contact" class="px-5 py-2 rounded-full border border-border-primary bg-btn-bg text-text-secondary hover:bg-btn-hover font-semibold font-sans text-xs transition-all btn-magnetic cursor-pointer shadow-sm">
              Contact
            </a>
          </div>
        </div>

      </div>

      <!-- Compact Values Row (Sitting right inside hero panel) -->
      <!-- <div class="grid grid-cols-1 sm:grid-cols-3 gap-4 pt-6 text-xs">
        <div class="space-y-1">
          <h4 class="text-brand-cyan font-bold font-sans flex items-center gap-1 justify-center sm:justify-start">
            <span>✔</span> User-Focus
          </h4>
          <p class="text-text-secondary leading-normal font-sans text-center sm:text-left">
            Designing code that saves clinic doctors and marketplace merchants valuable daily hours.
          </p>
        </div>
        
        <div class="space-y-1">
          <h4 class="text-brand-purple font-bold font-sans flex items-center gap-1 justify-center sm:justify-start">
            <span>🤝</span> Team Collaboration
          </h4>
          <p class="text-text-secondary leading-normal font-sans text-center sm:text-left">
            Maintaining direct, friendly alignment with product managers, designers, and web leads.
          </p>
        </div>

        <div class="space-y-1">
          <h4 class="text-brand-emerald font-bold font-sans flex items-center gap-1 justify-center sm:justify-start">
            <span>⛁</span> Clean Quality
          </h4>
          <p class="text-text-secondary leading-normal font-sans text-center sm:text-left">
            Writing tested, containerised software running smoothly in production under active user loads.
          </p>
        </div>
      </div> -->
    </div>
  </section>


  <!-- FEATURED PROJECTS SECTION -->
  <section id="projects" class="w-full max-w-5xl mx-auto px-4 py-8 reveal-item">
    <!-- Section Title -->
    <div class="text-center md:text-left mb-8">
      <div class="inline-flex items-center gap-2 px-3 py-1 rounded-full border border-brand-cyan/20 bg-brand-cyan/5 mb-3">
        <span class="w-2 h-2 rounded-full bg-brand-cyan animate-pulse"></span>
        <span class="text-xs font-mono text-brand-cyan uppercase tracking-widest font-semibold">Showcases</span>
      </div>
      <h2 class="text-2xl md:text-3xl font-sans font-bold text-text-white tracking-tight">
        Featured Projects
      </h2>
      <p class="text-text-secondary font-mono text-xs max-w-xl mt-1 leading-relaxed">
        Real-world applications delivering measurable business outcomes and operational value.
      </p>
    </div>

    <!-- Projects Grid (Spacious symmetrical card view) -->
    <div class="grid grid-cols-1 lg:grid-cols-2 gap-8">
      {#each projects as proj}
        <ProjectCard project={proj} />
      {/each}
    </div>
  </section>

  <!-- TECH STACK SECTION (Compact Grid Cloud) -->
  <section class="w-full max-w-5xl mx-auto py-4 reveal-item">
    <TechOrbit />
  </section>

  <!-- EXPERIENCE TIMELINE SECTION (Compact timeline) -->
  <section class="w-full max-w-5xl mx-auto py-4 reveal-item">
    <ExperienceTimeline />
  </section>


  <!-- CONTACT SECTION (Compact Grid Card) -->
  <section id="contact" class="w-full max-w-5xl mx-auto px-4 py-12 reveal-item">
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
          
          <h2 class="text-2xl md:text-3xl font-sans font-bold text-text-white tracking-tight leading-tight">
            Let's build together.
          </h2>
          
          <p class="text-text-secondary text-xs leading-relaxed font-sans">
            Interested in scaling systems or have an open role on your developer team? Let's discuss directly.
          </p>

          <div class="space-y-3 pt-3 border-t border-border-primary">
            <a href="mailto:m.jawahiruzzaman@gmail.com" class="flex items-center gap-2.5 text-text-primary hover:text-brand-cyan transition-colors text-xs font-mono group cursor-pointer">
              <span class="w-7 h-7 rounded-lg bg-bg-secondary border border-border-primary flex items-center justify-center font-bold text-text-secondary group-hover:text-brand-cyan">@</span>
              <span>m.jawahiruzzaman@gmail.com</span>
            </a>
            
            <a href="tel:+6285728888280" class="flex items-center gap-2.5 text-text-primary hover:text-brand-cyan transition-colors text-xs font-mono group cursor-pointer">
              <span class="w-7 h-7 rounded-lg bg-bg-secondary border border-border-primary flex items-center justify-center font-bold text-text-secondary group-hover:text-brand-cyan">☏</span>
              <span>+6285728888280</span>
            </a>

            <!-- Social Links Row -->
            <div class="flex gap-3 pt-1">
              <a href="https://github.com" target="_blank" rel="noopener noreferrer" class="px-3 py-1.5 rounded-lg border border-border-primary bg-bg-secondary font-mono text-[9px] text-text-secondary hover:text-text-white hover:border-border-primary transition-all cursor-pointer">
                GITHUB
              </a>
              <a href="https://linkedin.com" target="_blank" rel="noopener noreferrer" class="px-3 py-1.5 rounded-lg border border-border-primary bg-bg-secondary font-mono text-[9px] text-text-secondary hover:text-text-white hover:border-border-primary transition-all cursor-pointer">
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
                <label for="name" class="block text-text-secondary font-mono text-[10px] uppercase mb-1">Your Name</label>
                <input id="name" type="text" bind:value={contactName} required placeholder="Full Name" class="w-full bg-bg-secondary border border-border-primary rounded-xl px-3 py-2 text-text-white placeholder-text-muted focus:border-brand-cyan focus:outline-none transition-all focus:ring-1 focus:ring-brand-cyan/20" />
              </div>
              <div>
                <label for="email" class="block text-text-secondary font-mono text-[10px] uppercase mb-1">Your Email</label>
                <input id="email" type="email" bind:value={contactEmail} required placeholder="email@domain.com" class="w-full bg-bg-secondary border border-border-primary rounded-xl px-3 py-2 text-text-white placeholder-text-muted focus:border-brand-cyan focus:outline-none transition-all focus:ring-1 focus:ring-brand-cyan/20" />
              </div>
            </div>
            <div>
              <label for="message" class="block text-text-secondary font-mono text-[10px] uppercase mb-1">Your Message</label>
              <textarea id="message" bind:value={contactMessage} required rows="3" placeholder="Compose message..." class="w-full bg-bg-secondary border border-border-primary rounded-xl px-3 py-2 text-text-white placeholder-text-muted focus:border-brand-cyan focus:outline-none transition-all resize-none focus:ring-1 focus:ring-brand-cyan/20"></textarea>
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
