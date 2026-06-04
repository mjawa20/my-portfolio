<script lang="ts">
  interface Testimonial {
    quote: string;
    author: string;
    role: string;
    org: string;
    avatarInitials: string;
    themeColor: string;
  }

  const testimonials: Testimonial[] = [
    {
      quote: "Jawahir took time to understand our clinical workflows. He optimized the EMR bridging system so doctors can verify insurance eligibility instantly. His work cut registration waiting lines in half and resolved patient check-in mismatches. He is highly capable and communicates technical plans in clear, operational terms.",
      author: "Dr. Farhan H.",
      role: "Clinical Operations Director",
      org: "Partner Medical Center",
      avatarInitials: "FH",
      themeColor: "from-cyan-500 to-blue-500"
    },
    {
      quote: "Working with Jawahir was a breeze. He structured our marketplace sync pipelines, ensuring catalog inventory updates across multiple channels in under 2 seconds. He understands product priorities, aligns well with business goals, and is a proactive, friendly collaborator.",
      author: "Aditya Pratama",
      role: "Product Manager",
      org: "ToKonek Omnichannel SaaS",
      avatarInitials: "AP",
      themeColor: "from-purple-500 to-indigo-500"
    },
    {
      quote: "Jawahir has a rare capability to bridge robust backend logic with smooth user experiences. He collaborated closely with our team during interface integration, optimizing GraphQL endpoints and aligning frontends. Highly dependable and a fantastic team player.",
      author: "Maya Lestari",
      role: "Lead Frontend Engineer",
      org: "Mitra Sinerji Teknoindo",
      avatarInitials: "ML",
      themeColor: "from-emerald-500 to-teal-500"
    }
  ];
</script>

<div class="w-full max-w-7xl mx-auto px-4 py-16" id="testimonials">
  
  <!-- Section Title -->
  <div class="text-center md:text-left mb-12">
    <div class="inline-flex items-center gap-2 px-3 py-1 rounded-full border border-brand-cyan/20 bg-brand-cyan/5 mb-4">
      <span class="w-2 h-2 rounded-full bg-brand-cyan animate-pulse"></span>
      <span class="text-xs font-mono text-brand-cyan uppercase tracking-widest font-semibold">Collaboration Feedbacks</span>
    </div>
    <h2 class="text-3xl md:text-4xl font-sans font-bold text-white tracking-tight">
      What Collaborators Say
    </h2>
    <p class="text-neutral-400 font-mono text-sm max-w-2xl mt-2 leading-relaxed">
      A reflection of professional reliability, team collaboration, and the impact delivered across client environments.
    </p>
  </div>

  <!-- Testimonials Grid -->
  <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
    {#each testimonials as item}
      <div class="glass-panel relative rounded-2xl p-6 md:p-8 flex flex-col justify-between hover:border-white/20 transition-all duration-300 group">
        
        <!-- Quote Icon decoration -->
        <span class="text-neutral-800 text-6xl font-serif absolute right-6 top-4 select-none leading-none group-hover:text-brand-cyan/10 transition-colors pointer-events-none">“</span>
        
        <!-- Quote Text -->
        <p class="text-neutral-300 text-sm leading-relaxed mb-8 italic relative z-10 font-sans">
          "{item.quote}"
        </p>

        <!-- Profile details -->
        <div class="flex items-center gap-4 border-t border-white/5 pt-4">
          <!-- Avatar Icon -->
          <div class="w-10 h-10 rounded-full bg-gradient-to-br {item.themeColor} flex items-center justify-center text-white font-mono text-sm font-bold shadow-[0_0_10px_rgba(0,0,0,0.5)]">
            {item.avatarInitials}
          </div>
          <div>
            <h4 class="text-white font-bold text-sm font-sans">{item.author}</h4>
            <p class="text-neutral-500 text-[11px] font-mono leading-tight">
              {item.role} <span class="text-neutral-600">//</span> {item.org}
            </p>
          </div>
        </div>

      </div>
    {/each}
  </div>

</div>
