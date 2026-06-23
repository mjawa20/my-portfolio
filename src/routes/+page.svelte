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
    link.href = '/cv.pdf';
    link.download = 'cv.pdf';

    document.body.appendChild(link);
    link.click();
    document.body.removeChild(link);

    triggerToast('✔ Download complete.');
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
      images: ['/wordrush.png', '/wordrush5.jpg', '/wordrush2.jpg', '/wordrush3.jpg', '/wordrush4.jpg']
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
      images: ['/project_emr_mockup.png', '/project_emr_mockup_2.png']
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
      images: [ '/tokonek3.png', '/tokonek.png', '/tokonek2.png',  '/tokonek4.png',]
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
      link: 'https://sertifikasi.postel.go.id/',
      images: ['/sertifikasi.png']
    }
  ];

  // Contact Form State
  let contactName = $state('');
  let contactEmail = $state('');
  let contactMessage = $state('');
  let isSending = $state(false);
  let formSent = $state(false);

  // Web3Forms Access Key - Dapatkan gratis di https://web3forms.com
  // Masukkan key Anda di sini untuk mengirim email langsung di background.
  // Jika dikosongkan, form akan otomatis menggunakan mailto (membuka aplikasi email).
  let web3formsKey = $state('b7977135-1ead-4466-be82-b16c9136df5c');

  async function handleContactSubmit(e: SubmitEvent) {
    e.preventDefault();
    if (!contactName || !contactEmail || !contactMessage) return;

    isSending = true;

    if (web3formsKey) {
      // Kirim via Web3Forms API (Background / Silent)
      try {
        const response = await fetch('https://api.web3forms.com/submit', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
            'Accept': 'application/json'
          },
          body: JSON.stringify({
            access_key: web3formsKey,
            name: contactName,
            email: contactEmail,
            message: contactMessage,
            subject: `Portfolio Message from ${contactName}`
          })
        });

        const result = await response.json();
        if (result.success) {
          formSent = true;
          triggerToast('✔ Message sent successfully!');
          contactName = '';
          contactEmail = '';
          contactMessage = '';
        } else {
          throw new Error(result.message || 'Submission failed');
        }
      } catch (err) {
        console.error(err);
        triggerToast('❌ Error. Opening email client...');
        triggerMailto();
      } finally {
        isSending = false;
      }
    } else {
      // Fallback ke Mailto (Synchronous - langsung terbuka tanpa diblokir browser)
      triggerMailto();
      isSending = false;
      formSent = true;
    }
  }

  function triggerMailto() {
    const recipient = 'm.jawahiruzzaman@gmail.com';
    const subject = encodeURIComponent(`Message from Portfolio - ${contactName}`);
    const body = encodeURIComponent(`Hello,\n\nYou received a new message from your portfolio contact form:\n\nName: ${contactName}\nEmail: ${contactEmail}\n\nMessage:\n${contactMessage}\n\n---\nSent from Portfolio Website`);
    
    const mailtoUrl = `mailto:${recipient}?subject=${subject}&body=${body}`;
    const link = document.createElement('a');
    link.href = mailtoUrl;
    link.target = '_blank';
    link.rel = 'noopener noreferrer';
    document.body.appendChild(link);
    link.click();
    document.body.removeChild(link);
    
    triggerToast('📧 Opening email client...');
    
    contactName = '';
    contactEmail = '';
    contactMessage = '';
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
          
          <!-- Interactive Themed Brand Icon Row -->
          <div class="flex items-center gap-2.5 mt-3.5">
            <a 
              href="https://github.com/mjawa20" 
              target="_blank" 
              rel="noopener noreferrer" 
              class="w-8 h-8 rounded-lg border border-border-primary bg-bg-secondary hover:bg-btn-hover text-text-secondary hover:text-white flex items-center justify-center transition-all cursor-pointer hover:scale-105 active:scale-95 shadow-sm group/btn hover:border-brand-purple/40 hover:shadow-[0_0_12px_rgba(139,92,246,0.15)]"
              aria-label="GitHub Profile"
            >
              <svg class="w-4 h-4 transition-transform duration-300 group-hover/btn:scale-110" fill="currentColor" viewBox="0 0 24 24"><path d="M12 .297c-6.63 0-12 5.373-12 12 0 5.303 3.438 9.8 8.205 11.385.6.113.82-.258.82-.577 0-.285-.01-1.04-.015-2.04-3.338.724-4.042-1.61-4.042-1.61C4.422 18.07 3.633 17.7 3.633 17.7c-1.087-.744.084-.729.084-.729 1.205.084 1.838 1.236 1.838 1.236 1.07 1.835 2.809 1.305 3.495.998.108-.776.417-1.305.76-1.605-2.665-.3-5.466-1.332-5.466-5.93 0-1.31.465-2.38 1.235-3.22-.135-.303-.54-1.523.105-3.176 0 0 1.005-.322 3.3 1.23.96-.267 1.98-.399 3-.405 1.02.006 2.04.138 3 .405 2.28-1.552 3.285-1.23 3.285-1.23.645 1.653.24 2.873.12 3.176.765.84 1.23 1.91 1.23 3.22 0 4.61-2.805 5.625-5.475 5.92.42.36.81 1.096.81 2.22 0 1.606-.015 2.896-.015 3.286 0 .315.21.69.825.57C20.565 22.092 24 17.592 24 12.297c0-6.627-5.373-12-12-12"/></svg>
            </a>
            
            <a 
              href="https://linkedin.com/in/amanwp/" 
              target="_blank" 
              rel="noopener noreferrer" 
              class="w-8 h-8 rounded-lg border border-border-primary bg-bg-secondary hover:bg-btn-hover text-text-secondary hover:text-brand-blue flex items-center justify-center transition-all cursor-pointer hover:scale-105 active:scale-95 shadow-sm group/btn hover:border-brand-blue/40 hover:shadow-[0_0_12px_rgba(59,130,246,0.15)]"
              aria-label="LinkedIn Profile"
            >
              <svg class="w-4 h-4 transition-transform duration-300 group-hover/btn:scale-110" fill="currentColor" viewBox="0 0 24 24"><path d="M19 0h-14c-2.761 0-5 2.239-5 5v14c0 2.761 2.239 5 5 5h14c2.762 0 5-2.239 5-5v-14c0-2.761-2.238-5-5-5zm-11 19h-3v-11h3v11zm-1.5-12.268c-.966 0-1.75-.779-1.75-1.75s.784-1.75 1.75-1.75 1.75.779 1.75 1.75-.784 1.75-1.75 1.75zm13.5 12.268h-3v-5.604c0-3.368-4-3.113-4 0v5.604h-3v-11h3v1.765c1.396-2.586 7-2.777 7 2.476v6.759z"/></svg>
            </a>

            <a 
              href="mailto:m.jawahiruzzaman@gmail.com" 
              class="w-8 h-8 rounded-lg border border-border-primary bg-bg-secondary hover:bg-btn-hover text-text-secondary hover:text-brand-cyan flex items-center justify-center transition-all cursor-pointer hover:scale-105 active:scale-95 shadow-sm group/btn hover:border-brand-cyan/40 hover:shadow-[0_0_12px_rgba(6,182,212,0.15)]"
              aria-label="Send Email"
            >
              <svg class="w-4 h-4 transition-transform duration-300 group-hover/btn:scale-110" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"></path><polyline points="22,6 12,13 2,6"></polyline></svg>
            </a>

            <a 
              href="https://wa.me/6285728888280" 
              target="_blank"
              rel="noopener noreferrer"
              class="w-8 h-8 rounded-lg border border-border-primary bg-bg-secondary hover:bg-btn-hover text-text-secondary hover:text-brand-emerald flex items-center justify-center transition-all cursor-pointer hover:scale-105 active:scale-95 shadow-sm group/btn hover:border-brand-emerald/40 hover:shadow-[0_0_12px_rgba(16,185,129,0.15)]"
              aria-label="Chat via WhatsApp"
            >
              <svg class="w-4 h-4 transition-transform duration-300 group-hover/btn:scale-110" fill="currentColor" viewBox="0 0 24 24"><path d="M12.004 2C6.48 2 2 6.48 2 12.004c0 1.762.46 3.478 1.334 5l-1.42 5.19 5.302-1.39A9.97 9.97 0 0 0 12.004 22c5.524 0 10.004-4.48 10.004-10.004C22.008 6.48 17.528 2 12.004 2zm6.262 14.188c-.26.733-1.49 1.403-2.072 1.488-.53.078-1.22.146-3.864-.897-3.38-1.332-5.56-4.777-5.73-5.003-.17-.226-1.378-1.833-1.378-3.497 0-1.664.87-2.48 1.18-2.812.31-.33.68-.415.9-.415.222 0 .445.002.637.012.2.01.467-.04.726.586.263.637.893 2.183.97 2.342.078.16.13.346.023.56-.106.213-.16.347-.32.532-.16.186-.337.416-.48.56-.16.16-.328.332-.14.654.187.323.834 1.372 1.783 2.222.95.85 1.75 1.112 2.074 1.274.323.16.51.135.7-.08.19-.214.814-.947 1.03-1.272.217-.324.433-.27.732-.16.3.11 1.902.898 2.228 1.06.326.164.542.245.62.38.077.135.077.785-.183 1.518z"/></svg>
            </a>
          </div>
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
            Software Engineer with 3+ years of experience building production-grade applications across healthcare and business domains. Skilled in PHP, Go, JavaScript, and TypeScript, with hands-on experience in Phalcon, Vue.js, SvelteKit, and NestJS. Experienced in developing scalable web applications, RESTful APIs, and integrating healthcare systems such as BPJS, Laboratory Information Systems (LIS), and Radiology Information Systems (RIS). Passionate about delivering reliable, maintainable, and user-focused solutions while collaborating effectively within Agile teams. 
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

          <div class="flex flex-col gap-3 pt-3.5 border-t border-border-primary">
            
            <!-- Email Contact Pill -->
            <a 
              href="mailto:m.jawahiruzzaman@gmail.com" 
              class="glass-panel p-3.5 rounded-2xl flex items-center gap-3.5 hover:border-brand-cyan/45 hover:shadow-[0_8px_30px_rgba(6,182,212,0.12)] hover:translate-x-1.5 transition-all duration-300 group cursor-pointer"
              style:box-shadow="var(--card-shadow)"
            >
              <span class="w-9 h-9 rounded-xl bg-brand-cyan/10 border border-brand-cyan/20 flex items-center justify-center text-brand-cyan group-hover:scale-110 group-hover:bg-brand-cyan/20 transition-all duration-300 shrink-0">
                <svg class="w-4 h-4" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"></path><polyline points="22,6 12,13 2,6"></polyline></svg>
              </span>
              <div class="flex flex-col min-w-0">
                <span class="text-[9px] font-mono text-brand-cyan font-bold tracking-wider uppercase leading-none mb-1">Email</span>
                <span class="text-xs text-text-primary group-hover:text-brand-cyan transition-colors block font-mono break-all sm:break-normal">m.jawahiruzzaman@gmail.com</span>
              </div>
            </a>

            <!-- Phone / WA Contact Pill -->
            <a 
              href="https://wa.me/6285728888280" 
              target="_blank"
              rel="noopener noreferrer"
              class="glass-panel p-3.5 rounded-2xl flex items-center gap-3.5 hover:border-brand-emerald/45 hover:shadow-[0_8px_30px_rgba(16,185,129,0.12)] hover:translate-x-1.5 transition-all duration-300 group cursor-pointer"
              style:box-shadow="var(--card-shadow)"
            >
              <span class="w-9 h-9 rounded-xl bg-brand-emerald/10 border border-brand-emerald/20 flex items-center justify-center text-brand-emerald group-hover:scale-110 group-hover:bg-brand-emerald/20 transition-all duration-300 shrink-0">
                <svg class="w-4 h-4" fill="currentColor" viewBox="0 0 24 24"><path d="M12.004 2C6.48 2 2 6.48 2 12.004c0 1.762.46 3.478 1.334 5l-1.42 5.19 5.302-1.39A9.97 9.97 0 0 0 12.004 22c5.524 0 10.004-4.48 10.004-10.004C22.008 6.48 17.528 2 12.004 2zm6.262 14.188c-.26.733-1.49 1.403-2.072 1.488-.53.078-1.22.146-3.864-.897-3.38-1.332-5.56-4.777-5.73-5.003-.17-.226-1.378-1.833-1.378-3.497 0-1.664.87-2.48 1.18-2.812.31-.33.68-.415.9-.415.222 0 .445.002.637.012.2.01.467-.04.726.586.263.637.893 2.183.97 2.342.078.16.13.346.023.56-.106.213-.16.347-.32.532-.16.186-.337.416-.48.56-.16.16-.328.332-.14.654.187.323.834 1.372 1.783 2.222.95.85 1.75 1.112 2.074 1.274.323.16.51.135.7-.08.19-.214.814-.947 1.03-1.272.217-.324.433-.27.732-.16.3.11 1.902.898 2.228 1.06.326.164.542.245.62.38.077.135.077.785-.183 1.518z"/></svg>
              </span>
              <div class="flex flex-col min-w-0">
                <span class="text-[9px] font-mono text-brand-emerald font-bold tracking-wider uppercase leading-none mb-1">WhatsApp</span>
                <span class="text-xs text-text-primary group-hover:text-brand-emerald transition-colors block font-mono break-all sm:break-normal">+6285728888280</span>
              </div>
            </a>

            <!-- GitHub Contact Pill -->
            <a 
              href="https://github.com/mjawa20" 
              target="_blank" 
              rel="noopener noreferrer" 
              class="glass-panel p-3.5 rounded-2xl flex items-center gap-3.5 hover:border-brand-purple/45 hover:shadow-[0_8px_30px_rgba(139,92,246,0.12)] hover:translate-x-1.5 transition-all duration-300 group cursor-pointer"
              style:box-shadow="var(--card-shadow)"
            >
              <span class="w-9 h-9 rounded-xl bg-brand-purple/10 border border-brand-purple/20 flex items-center justify-center text-brand-purple group-hover:scale-110 group-hover:bg-brand-purple/20 transition-all duration-300 shrink-0">
                <svg class="w-4 h-4" fill="currentColor" viewBox="0 0 24 24"><path d="M12 .297c-6.63 0-12 5.373-12 12 0 5.303 3.438 9.8 8.205 11.385.6.113.82-.258.82-.577 0-.285-.01-1.04-.015-2.04-3.338.724-4.042-1.61-4.042-1.61C4.422 18.07 3.633 17.7 3.633 17.7c-1.087-.744.084-.729.084-.729 1.205.084 1.838 1.236 1.838 1.236 1.07 1.835 2.809 1.305 3.495.998.108-.776.417-1.305.76-1.605-2.665-.3-5.466-1.332-5.466-5.93 0-1.31.465-2.38 1.235-3.22-.135-.303-.54-1.523.105-3.176 0 0 1.005-.322 3.3 1.23.96-.267 1.98-.399 3-.405 1.02.006 2.04.138 3 .405 2.28-1.552 3.285-1.23 3.285-1.23.645 1.653.24 2.873.12 3.176.765.84 1.23 1.91 1.23 3.22 0 4.61-2.805 5.625-5.475 5.92.42.36.81 1.096.81 2.22 0 1.606-.015 2.896-.015 3.286 0 .315.21.69.825.57C20.565 22.092 24 17.592 24 12.297c0-6.627-5.373-12-12-12"/></svg>
              </span>
              <div class="flex flex-col min-w-0">
                <span class="text-[9px] font-mono text-brand-purple font-bold tracking-wider uppercase leading-none mb-1">GitHub</span>
                <span class="text-xs text-text-primary group-hover:text-brand-purple transition-colors block font-mono break-all sm:break-normal">github.com/mjawa20</span>
              </div>
            </a>

            <!-- LinkedIn Contact Pill -->
            <a 
              href="https://linkedin.com/in/amanwp/" 
              target="_blank" 
              rel="noopener noreferrer" 
              class="glass-panel p-3.5 rounded-2xl flex items-center gap-3.5 hover:border-brand-blue/45 hover:shadow-[0_8px_30px_rgba(59,130,246,0.12)] hover:translate-x-1.5 transition-all duration-300 group cursor-pointer"
              style:box-shadow="var(--card-shadow)"
            >
              <span class="w-9 h-9 rounded-xl bg-brand-blue/10 border border-brand-blue/20 flex items-center justify-center text-brand-blue group-hover:scale-110 group-hover:bg-brand-blue/20 transition-all duration-300 shrink-0">
                <svg class="w-4 h-4" fill="currentColor" viewBox="0 0 24 24"><path d="M19 0h-14c-2.761 0-5 2.239-5 5v14c0 2.761 2.239 5 5 5h14c2.762 0 5-2.239 5-5v-14c0-2.761-2.238-5-5-5zm-11 19h-3v-11h3v11zm-1.5-12.268c-.966 0-1.75-.779-1.75-1.75s.784-1.75 1.75-1.75 1.75.779 1.75 1.75-.784 1.75-1.75 1.75zm13.5 12.268h-3v-5.604c0-3.368-4-3.113-4 0v5.604h-3v-11h3v1.765c1.396-2.586 7-2.777 7 2.476v6.759z"/></svg>
              </span>
              <div class="flex flex-col min-w-0">
                <span class="text-[9px] font-mono text-brand-blue font-bold tracking-wider uppercase leading-none mb-1">LinkedIn</span>
                <span class="text-xs text-text-primary group-hover:text-brand-blue transition-colors block font-mono break-all sm:break-normal">linkedin.com/in/amanwp/</span>
              </div>
            </a>

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
