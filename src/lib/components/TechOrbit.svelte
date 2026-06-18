<script lang="ts">
  interface Skill {
    name: string;
    iconType: 'devicon' | 'simpleicon' | 'custom';
    iconValue: string;
    brandColor: string;
    description: string;
  }

  interface TechCategory {
    title: string;
    description: string;
    themeClass: string;
    dotClass: string;
    colSpanClass: string;
    gridCols: string;
    skills: Skill[];
  }

  const categories: TechCategory[] = [
    {
      title: "Languages",
      description: "Programming languages used for building applications.",
      themeClass: "border-brand-cyan/20 bg-brand-cyan/5 text-brand-cyan",
      dotClass: "bg-brand-cyan",
      colSpanClass: "col-span-1 md:col-span-1 lg:col-span-2",
      gridCols: "grid-cols-1",
      skills: [
        {
          name: "PHP",
          iconType: "devicon",
          iconValue: "devicon-php-plain",
          brandColor: "#777BB4",
          description: "Main language for Electronic Medical Record (EMR) system."
        },
        {
          name: "JavaScript",
          iconType: "devicon",
          iconValue: "devicon-javascript-plain",
          brandColor: "#F7DF1E",
          description: "Core scripting for web interactive features and mobile games."
        },
        {
          name: "TypeScript",
          iconType: "devicon",
          iconValue: "devicon-typescript-plain",
          brandColor: "#3178C6",
          description: "Used in Nuxt.js, SvelteKit, and NestJS backends."
        },
        {
          name: "Go",
          iconType: "devicon",
          iconValue: "devicon-go-plain",
          brandColor: "#00ADD8",
          description: "Engineered high-performance backend services & REST/GraphQL APIs."
        },
        {
          name: "Dart",
          iconType: "devicon",
          iconValue: "devicon-dart-plain",
          brandColor: "#0175C2",
          description: "Developed mobile games using Flutter & Flame Engine."
        }
      ]
    },
    {
      title: "Databases & Storage",
      description: "Structured storage engines and cache registries.",
      themeClass: "border-brand-emerald/20 bg-brand-emerald/5 text-brand-emerald",
      dotClass: "bg-brand-emerald",
      colSpanClass: "col-span-1 md:col-span-1 lg:col-span-2",
      gridCols: "grid-cols-1",
      skills: [
        {
          name: "PostgreSQL",
          iconType: "devicon",
          iconValue: "devicon-postgresql-plain",
          brandColor: "#4169E1",
          description: "Primary database for Tokonek, E-Sertifikasi, and Kominfo apps."
        },
        {
          name: "MariaDB",
          iconType: "devicon",
          iconValue: "devicon-mariadb-original",
          brandColor: "#003545",
          description: "Managed relational database schemas and queries."
        },
        {
          name: "MongoDB",
          iconType: "devicon",
          iconValue: "devicon-mongodb-plain",
          brandColor: "#47A248",
          description: "Used for document storage and NoSQL requirements."
        },
        {
          name: "Redis",
          iconType: "devicon",
          iconValue: "devicon-redis-plain",
          brandColor: "#FF3E3E",
          description: "Caching and real-time state for EMR and Tokonek systems."
        },
        {
          name: "MinIO",
          iconType: "simpleicon",
          iconValue: "minio",
          brandColor: "#C72C48",
          description: "Object storage for files in the Tokonek omnichannel SaaS."
        }
      ]
    },
    {
      title: "Tools & DevOps",
      description: "Integration pipelines, cloud, and environment tools.",
      themeClass: "border-brand-blue/20 bg-brand-blue/5 text-brand-blue",
      dotClass: "bg-brand-blue",
      colSpanClass: "col-span-1 md:col-span-1 lg:col-span-2",
      gridCols: "grid-cols-1",
      skills: [
        {
          name: "Docker",
          iconType: "devicon",
          iconValue: "devicon-docker-plain",
          brandColor: "#2496ED",
          description: "Containerized backend services and web applications."
        },
        {
          name: "Git",
          iconType: "devicon",
          iconValue: "devicon-git-plain",
          brandColor: "#F05032",
          description: "Version control for agile collaboration and git-flow pipelines."
        },
        {
          name: "Firebase",
          iconType: "devicon",
          iconValue: "devicon-firebase-plain",
          brandColor: "#FFCA28",
          description: "Firestore and auth features for WordRush mobile game."
        }
      ]
    },
    {
      title: "Front End",
      description: "Responsive frameworks and user interface libraries.",
      themeClass: "border-brand-purple/20 bg-brand-purple/5 text-brand-purple",
      dotClass: "bg-brand-purple",
      colSpanClass: "col-span-1 md:col-span-1 lg:col-span-3",
      gridCols: "grid-cols-1 sm:grid-cols-2",
      skills: [
        {
          name: "Vue.js",
          iconType: "devicon",
          iconValue: "devicon-vuejs-plain",
          brandColor: "#4FC08D",
          description: "Implemented EMR dashboard and E-Sertifikasi portals."
        },
        {
          name: "SvelteKit",
          iconType: "devicon",
          iconValue: "devicon-svelte-plain",
          brandColor: "#FF3E00",
          description: "Main framework for Tokonek SaaS platform & this portfolio."
        },
        {
          name: "Nuxt.js",
          iconType: "devicon",
          iconValue: "devicon-nuxtjs-plain",
          brandColor: "#00DC82",
          description: "Built responsive web interfaces and Kominfo applications."
        },
        {
          name: "React.js",
          iconType: "devicon",
          iconValue: "devicon-react-original",
          brandColor: "#61DAFB",
          description: "Skilled in component-based web development."
        },
        {
          name: "Flutter",
          iconType: "devicon",
          iconValue: "devicon-flutter-plain",
          brandColor: "#02569B",
          description: "Created games like WordRush and custom mobile apps."
        },
        {
          name: "Tailwind CSS",
          iconType: "devicon",
          iconValue: "devicon-tailwindcss-plain",
          brandColor: "#06B6D4",
          description: "Main styling utility across Nuxt, Svelte, and React projects."
        },
        {
          name: "Pinia",
          iconType: "simpleicon",
          iconValue: "pinia",
          brandColor: "#FFE066",
          description: "Managed reactive state in Vue.js applications."
        },
        {
          name: "Redux",
          iconType: "devicon",
          iconValue: "devicon-redux-original",
          brandColor: "#764ABC",
          description: "Experienced in state management for complex React apps."
        },
        {
          name: "HTML",
          iconType: "devicon",
          iconValue: "devicon-html5-plain",
          brandColor: "#E34F26",
          description: "Standard structures for modern responsive templates."
        },
        {
          name: "CSS",
          iconType: "devicon",
          iconValue: "devicon-css3-plain",
          brandColor: "#1572B6",
          description: "Custom animations, grid layouts, and visual polishing."
        }
      ]
    },
    {
      title: "Back End",
      description: "Server architecture, APIs, and microservices.",
      themeClass: "border-brand-cyan/20 bg-brand-cyan/5 text-brand-cyan",
      dotClass: "bg-brand-cyan",
      colSpanClass: "col-span-1 md:col-span-2 lg:col-span-3",
      gridCols: "grid-cols-1 sm:grid-cols-2",
      skills: [
        {
          name: "Phalcon",
          iconType: "devicon",
          iconValue: "devicon-phalcon-plain",
          brandColor: "#852B80",
          description: "Main PHP framework for the production EMR system at Zi.Care."
        },
        {
          name: "NestJS",
          iconType: "devicon",
          iconValue: "devicon-nestjs-plain",
          brandColor: "#E0234E",
          description: "Developed RESTful APIs with Clean Architecture at Japi AI."
        },
        {
          name: "Express.js",
          iconType: "devicon",
          iconValue: "devicon-express-original",
          brandColor: "#94a3b8",
          description: "Built microservices and lightweight Node backends."
        },
        {
          name: "Gin",
          iconType: "simpleicon",
          iconValue: "gin",
          brandColor: "#00ADD8",
          description: "Used for high-performance Golang APIs."
        },
        {
          name: "Laravel",
          iconType: "devicon",
          iconValue: "devicon-laravel-plain",
          brandColor: "#FF2D20",
          description: "Skilled in PHP application development and MVC design."
        },
        {
          name: "REST API",
          iconType: "custom",
          iconValue: "api",
          brandColor: "#06B6D4",
          description: "Created clean, documented interfaces for healthcare & business."
        },
        {
          name: "GraphQL",
          iconType: "devicon",
          iconValue: "devicon-graphql-plain",
          brandColor: "#E10098",
          description: "Implemented GraphQL APIs to optimize Kominfo app data flow."
        },
        {
          name: "RabbitMQ",
          iconType: "devicon",
          iconValue: "devicon-rabbitmq-original",
          brandColor: "#FF6600",
          description: "Message broker for distributed asynchronous queues."
        }
      ]
    }
  ];
</script>

<div class="w-full max-w-5xl mx-auto px-4 py-12" id="skills">
  
  <!-- Section Title -->
  <div class="text-center md:text-left mb-10">
    <div class="inline-flex items-center gap-2 px-3 py-1 rounded-full border border-brand-cyan/20 bg-brand-cyan/5 mb-3">
      <span class="w-2 h-2 rounded-full bg-brand-cyan animate-pulse"></span>
      <span class="text-xs font-mono text-brand-cyan uppercase tracking-widest font-semibold">Capabilities</span>
    </div>
    <h2 class="text-2xl md:text-3xl font-sans font-bold text-text-white tracking-tight">
      Technology Stack
    </h2>
    <p class="text-text-secondary font-mono text-xs max-w-xl mt-1 leading-relaxed">
      A structured summary of programming languages, libraries, databases, and tools categorized from my software engineering experience.
    </p>
  </div>

  <!-- Tech Grid Dashboard -->
  <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-6 gap-6">
    {#each categories as cat}
      <div class="glass-panel rounded-2xl p-5 hover:border-border-primary/50 transition-all duration-300 group flex flex-col justify-between {cat.colSpanClass}">
        <div>
          <!-- Header -->
          <div class="flex items-center justify-between mb-2 pb-2 border-b border-border-primary/40">
            <div class="flex items-center gap-2">
              <span class="w-1.5 h-1.5 rounded-full {cat.dotClass}"></span>
              <h3 class="text-xs sm:text-sm font-sans font-bold text-text-white tracking-tight">{cat.title}</h3>
            </div>
            <span class="text-[8px] font-mono px-2 py-0.5 rounded-full border {cat.themeClass}">Active</span>
          </div>
          
          <p class="text-text-muted text-[10px] font-sans mb-4 leading-relaxed">{cat.description}</p>
          
          <!-- Skill Tiles Grid -->
          <div class="grid gap-2.5 {cat.gridCols}">
            {#each cat.skills as skill}
              <div 
                class="skill-card flex items-center gap-2.5 p-2 rounded-xl border border-border-primary/55 bg-bg-secondary/20 relative overflow-hidden group/skill cursor-default"
                style="--hover-color: {skill.brandColor}"
              >
                <!-- Icon container -->
                <div class="w-7.5 h-7.5 shrink-0 flex items-center justify-center rounded-lg bg-bg-primary/50 border border-border-primary/50 group-hover/skill:border-[var(--hover-color)]/20 transition-all duration-300">
                  {#if skill.iconType === 'devicon'}
                    <i class="{skill.iconValue} colored text-[16px] sm:text-[18px] transition-transform duration-300 group-hover/skill:scale-110"></i>
                  {:else if skill.iconType === 'simpleicon'}
                    <img src="https://cdn.simpleicons.org/{skill.iconValue}" class="w-3.5 h-3.5 sm:w-4 sm:h-4 object-contain transition-transform duration-300 group-hover/skill:scale-110" alt="{skill.name}" />
                  {:else}
                    <!-- Custom SVG for REST API -->
                    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round" class="w-3.5 h-3.5 sm:w-4 sm:h-4 text-brand-cyan transition-transform duration-300 group-hover/skill:scale-110">
                      <polyline points="16 18 22 12 16 6"></polyline>
                      <polyline points="8 6 2 12 8 18"></polyline>
                    </svg>
                  {/if}
                </div>

                <!-- Info (Name & Description) -->
                <div class="flex flex-col min-w-0">
                  <span class="font-sans font-bold text-text-white text-[10.5px] sm:text-[11.5px] tracking-tight group-hover/skill:text-[var(--hover-color)] transition-colors duration-300">{skill.name}</span>
                  <span class="text-[9px] sm:text-[9.5px] text-text-secondary leading-normal mt-0.5 font-sans font-normal opacity-85 group-hover/skill:opacity-100 transition-opacity line-clamp-1">{skill.description}</span>
                </div>
              </div>
            {/each}
          </div>
        </div>
      </div>
    {/each}
  </div>

</div>

<style>
  .skill-card {
    transition: all 0.3s cubic-bezier(0.25, 0.8, 0.25, 1);
  }
  .skill-card:hover {
    border-color: var(--hover-color);
    box-shadow: 0 0 12px color-mix(in srgb, var(--hover-color) 16%, transparent);
    transform: translateY(-1.5px);
  }
</style>
