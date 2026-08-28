<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import {useToast} from "vue-toast-notification";

const isMobileMenuOpen = ref(false)
const currentYear = new Date().getFullYear()

const isSending = ref(false)
const isSuccess = ref(false)
const isError = ref(false)

const toast = useToast()

const submitForm = async (event) => {
  isSending.value = true
  isSuccess.value = false
  isError.value = false

  const form = event.target

  try {
    const response = await fetch('https://formspree.io/f/mgaeeowv', {
      method: 'POST',
      body: new FormData(form),
      headers: {
        Accept: 'application/json'
      }
    })

    if(response.ok)
    {
      isSuccess.value = true
      form.reset()
      toast.success('Thank you for reaching out. Your message has been ' +
          'successfully received. I’ll review it and get back to you shortly.', {
        position: 'top-right',
      })
    }
    else
    {
      isError.value = true
    }
  }
  catch (error)
  {
    isError.value = true
  }
  finally {
    isSending.value = false
  }
}

const scrollToSection = (id) => {
  isMobileMenuOpen.value = false
  const element = document.getElementById(id)
  if (element) {
    element.scrollIntoView({ behavior: 'smooth' })
  }
}

const handleResize = () => {
  if (window.innerWidth >= 768) {
    isMobileMenuOpen.value = false
  }
}

const handleEscape = (e) => {
  if (e.key === 'Escape' && isMobileMenuOpen.value) {
    isMobileMenuOpen.value = false
  }
}

onMounted(() => {
  window.addEventListener('resize', handleResize)
  document.addEventListener('keydown', handleEscape)
})

onUnmounted(() => {
  window.removeEventListener('resize', handleResize)
  document.removeEventListener('keydown', handleEscape)
})
</script>

<template>
  <div class="bg-[#0a0e17] text-gray-200 min-h-screen font-sans antialiased">
    <!-- Navigation -->
    <header class="fixed top-0 left-0 right-0 z-50 bg-[#0a0e17]/95 backdrop-blur-sm border-b border-gray-800/50">
      <nav class="max-w-6xl mx-auto px-4 sm:px-6 lg:px-8">
        <div class="flex justify-between items-center h-16">
          <!-- Logo -->
          <div class="flex flex-col leading-tight">
            <span class="text-white font-semibold text-lg tracking-tight">Djordje Maksimovic</span>
            <span class="text-xs text-blue-400/80 font-mono hidden sm:block">PHP / Laravel Developer</span>
          </div>

          <!-- Desktop Navigation -->
          <ul class="hidden md:flex space-x-8 text-sm font-medium">
            <li><button @click="scrollToSection('home')" class="hover:text-blue-400 transition-colors duration-200">Home</button></li>
            <li><button @click="scrollToSection('about')" class="hover:text-blue-400 transition-colors duration-200">About</button></li>
            <li><button @click="scrollToSection('skills')" class="hover:text-blue-400 transition-colors duration-200">Skills</button></li>
            <li><button @click="scrollToSection('experience')" class="hover:text-blue-400 transition-colors duration-200">Experience</button></li>
            <li><button @click="scrollToSection('projects')" class="hover:text-blue-400 transition-colors duration-200">Projects</button></li>
            <li><button @click="scrollToSection('contact')" class="hover:text-blue-400 transition-colors duration-200">Contact</button></li>
          </ul>

          <!-- Mobile menu button -->
          <button
              @click="isMobileMenuOpen = !isMobileMenuOpen"
              class="md:hidden text-gray-400 hover:text-white focus:outline-none"
              aria-label="Toggle menu"
              :aria-expanded="isMobileMenuOpen"
          >
            <svg v-if="!isMobileMenuOpen" class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16" />
            </svg>
            <svg v-else class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
            </svg>
          </button>
        </div>

        <!-- Mobile menu -->
        <div v-if="isMobileMenuOpen" class="md:hidden py-4 border-t border-gray-800/50">
          <ul class="flex flex-col space-y-3 text-sm font-medium">
            <li><button @click="scrollToSection('home')" class="block w-full text-left px-2 py-1 hover:text-blue-400 transition-colors duration-200">Home</button></li>
            <li><button @click="scrollToSection('about')" class="block w-full text-left px-2 py-1 hover:text-blue-400 transition-colors duration-200">About</button></li>
            <li><button @click="scrollToSection('skills')" class="block w-full text-left px-2 py-1 hover:text-blue-400 transition-colors duration-200">Skills</button></li>
            <li><button @click="scrollToSection('experience')" class="block w-full text-left px-2 py-1 hover:text-blue-400 transition-colors duration-200">Experience</button></li>
            <li><button @click="scrollToSection('projects')" class="block w-full text-left px-2 py-1 hover:text-blue-400 transition-colors duration-200">Projects</button></li>
            <li><button @click="scrollToSection('contact')" class="block w-full text-left px-2 py-1 hover:text-blue-400 transition-colors duration-200">Contact</button></li>
          </ul>
        </div>
      </nav>
    </header>

    <!-- Main Content -->
    <main>
      <!-- Hero Section -->
      <section id="home" class="pt-24 md:pt-32 pb-16 md:pb-24 px-4 sm:px-6 lg:px-8 max-w-6xl mx-auto min-h-screen flex items-center">
        <div class="grid md:grid-cols-2 gap-12 items-center w-full">
          <div>
            <p class="text-blue-400 font-mono text-sm mb-4">Hello, I'm</p>
            <h1 class="text-4xl sm:text-5xl lg:text-6xl font-bold text-white leading-tight">
              Djordje Maksimovic
            </h1>
            <p class="text-xl sm:text-2xl text-blue-400 font-medium mt-2">PHP / Laravel Developer</p>
            <p class="text-gray-400 text-lg mt-6 max-w-lg leading-relaxed">
              Building modern web applications with Laravel, Vue and MySQL.
              <span class="block mt-2 text-gray-400/80 text-base">
                4+ years of professional PHP experience working with both legacy systems and modern frameworks.
              </span>
            </p>
            <div class="flex flex-wrap gap-4 mt-8">
              <button @click="scrollToSection('projects')" class="px-6 py-3 bg-blue-600 hover:bg-blue-700 text-white font-medium rounded-md transition-colors duration-200">
                View Projects
              </button>
              <button @click="scrollToSection('contact')" class="px-6 py-3 border border-gray-700 hover:border-blue-500 text-gray-200 hover:text-white font-medium rounded-md transition-colors duration-200">
                Contact Me
              </button>
              <a href="https://github.com/MaximovicDj" target="_blank" rel="noopener noreferrer" class="px-6 py-3 bg-gray-800 hover:bg-gray-700 text-gray-200 font-medium rounded-md transition-colors duration-200 inline-flex items-center gap-2">
                <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 24 24">
                  <path d="M12 0C5.37 0 0 5.37 0 12c0 5.31 3.435 9.795 8.205 11.385.6.105.825-.255.825-.57 0-.285-.015-1.23-.015-2.235-3.015.555-3.795-.735-4.035-1.41-.135-.345-.72-1.41-1.23-1.695-.42-.225-1.02-.78-.015-.795.945-.015 1.62.87 1.845 1.23 1.08 1.815 2.805 1.305 3.495.99.105-.78.42-1.305.765-1.605-2.67-.3-5.46-1.335-5.46-5.925 0-1.305.465-2.385 1.23-3.225-.12-.3-.54-1.53.12-3.15 0 0 1.005-.315 3.3 1.23.96-.27 1.98-.405 3-.405s2.04.135 3 .405c2.295-1.56 3.3-1.23 3.3-1.23.66 1.62.24 2.85.12 3.15.765.84 1.23 1.905 1.23 3.225 0 4.605-2.805 5.625-5.475 5.925.435.375.81 1.095.81 2.22 0 1.605-.015 2.895-.015 3.3 0 .315.225.69.825.57A12.02 12.02 0 0024 12c0-6.63-5.37-12-12-12z" />
                </svg>
                GitHub
              </a>
            </div>
          </div>
          <div class="hidden md:flex justify-center items-center">
            <div class="w-full max-w-sm bg-[#111827] border border-gray-800 rounded-lg p-6 shadow-xl">
              <div class="flex items-center gap-2 text-gray-400 text-sm font-mono mb-4">
                <span class="w-3 h-3 rounded-full bg-red-500"></span>
                <span class="w-3 h-3 rounded-full bg-yellow-500"></span>
                <span class="w-3 h-3 rounded-full bg-green-500"></span>
                <span class="ml-2 text-gray-500">~/portfolio</span>
              </div>
              <div class="font-mono text-sm space-y-2">
                <p><span class="text-green-400">$</span> <span class="text-blue-300">php artisan serve</span></p>
                <p class="text-gray-400 text-xs">Laravel development server started</p>
                <p><span class="text-green-400">$</span> <span class="text-blue-300">npm run dev</span></p>
                <p class="text-gray-400 text-xs">Vite development server running</p>
                <p><span class="text-green-400">$</span> <span class="text-blue-300">git push origin main</span></p>
                <p class="text-gray-400 text-xs">Everything up-to-date</p>
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- About Section -->
      <section id="about" class="py-16 md:py-24 px-4 sm:px-6 lg:px-8 max-w-6xl mx-auto border-t border-gray-800/50">
        <h2 class="text-3xl md:text-4xl font-bold text-white mb-4">About Me</h2>
        <div class="grid md:grid-cols-5 gap-8 mt-8">
          <div class="md:col-span-3 space-y-4 text-gray-300 leading-relaxed">
            <p>
              I'm a <span class="text-blue-400 font-medium">PHP / Laravel Developer</span> with around 4 years of professional PHP experience.
              I currently work with both legacy PHP systems and modern Laravel/Vue applications.
            </p>
            <p>
              Working on a large legacy MVC/OOP PHP application has given me deep experience in
              <span class="text-blue-400 font-medium">debugging</span>, reading and understanding existing code,
              and maintaining production systems. I'm skilled at adding new functionality
              without breaking existing functionality.
            </p>
            <p>
              At the same time, I'm passionate about modern web development with
              <span class="text-blue-400 font-medium">Laravel</span>, <span class="text-blue-400 font-medium">Vue 3</span>,
              <span class="text-blue-400 font-medium">Inertia.js</span>, and <span class="text-blue-400 font-medium">Docker</span>.
              I enjoy building clean, maintainable applications and continuously improving my skills.
            </p>
          </div>
          <div class="md:col-span-2 grid grid-cols-2 gap-4">
            <div class="bg-[#111827] border border-gray-800 rounded-md p-4 text-center">
              <p class="text-3xl font-bold text-blue-400">4+</p>
              <p class="text-xs text-gray-400">Years PHP Experience</p>
            </div>
            <div class="bg-[#111827] border border-gray-800 rounded-md p-4 text-center">
              <p class="text-3xl font-bold text-blue-400">Laravel</p>
              <p class="text-xs text-gray-400">Main Framework</p>
            </div>
            <div class="bg-[#111827] border border-gray-800 rounded-md p-4 text-center">
              <p class="text-3xl font-bold text-blue-400">PHP</p>
              <p class="text-xs text-gray-400">Backend</p>
            </div>
            <div class="bg-[#111827] border border-gray-800 rounded-md p-4 text-center">
              <p class="text-3xl font-bold text-blue-400">Vue 3</p>
              <p class="text-xs text-gray-400">Frontend</p>
            </div>
          </div>
        </div>
      </section>

      <!-- Skills Section -->
      <section id="skills" class="py-16 md:py-24 px-4 sm:px-6 lg:px-8 max-w-6xl mx-auto border-t border-gray-800/50">
        <h2 class="text-3xl md:text-4xl font-bold text-white mb-4">Skills</h2>
        <div class="grid sm:grid-cols-2 lg:grid-cols-4 gap-6 mt-8">
          <!-- Backend -->
          <div class="bg-[#111827] border border-gray-800 rounded-md p-5">
            <h3 class="text-blue-400 font-semibold mb-3 text-sm uppercase tracking-wider">Backend</h3>
            <ul class="space-y-1.5 text-sm text-gray-300">
              <li>PHP</li>
              <li>Laravel</li>
              <li>REST APIs</li>
              <li>Laravel Sanctum</li>
              <li>Eloquent ORM</li>
              <li>Authentication</li>
              <li>Validation</li>
              <li>CRUD</li>
            </ul>
          </div>
          <!-- Frontend -->
          <div class="bg-[#111827] border border-gray-800 rounded-md p-5">
            <h3 class="text-blue-400 font-semibold mb-3 text-sm uppercase tracking-wider">Frontend</h3>
            <ul class="space-y-1.5 text-sm text-gray-300">
              <li>Vue 3</li>
              <li>JavaScript</li>
              <li>Inertia.js</li>
              <li>HTML</li>
              <li>CSS</li>
              <li>Tailwind CSS</li>
              <li>AJAX</li>
            </ul>
          </div>
          <!-- Database -->
          <div class="bg-[#111827] border border-gray-800 rounded-md p-5">
            <h3 class="text-blue-400 font-semibold mb-3 text-sm uppercase tracking-wider">Database</h3>
            <ul class="space-y-1.5 text-sm text-gray-300">
              <li>MySQL</li>
              <li>SQL</li>
              <li>Database Design</li>
              <li>Relationships</li>
              <li>Query Builder</li>
              <li>Eloquent</li>
            </ul>
          </div>
          <!-- Tools -->
          <div class="bg-[#111827] border border-gray-800 rounded-md p-5">
            <h3 class="text-blue-400 font-semibold mb-3 text-sm uppercase tracking-wider">Tools</h3>
            <ul class="space-y-1.5 text-sm text-gray-300">
              <li>Docker</li>
              <li>Git</li>
              <li>GitHub</li>
              <li>Composer</li>
              <li>NPM</li>
              <li>Vite</li>
            </ul>
          </div>
        </div>
      </section>

      <!-- Experience Section -->
      <section id="experience" class="py-16 md:py-24 px-4 sm:px-6 lg:px-8 max-w-6xl mx-auto border-t border-gray-800/50">
        <h2 class="text-3xl md:text-4xl font-bold text-white mb-4">Professional Experience</h2>

        <div class="mt-8">
          <!-- Main Experience Card -->
          <div class="bg-[#111827] border border-gray-800 rounded-md p-6">
            <div class="flex flex-wrap items-baseline justify-between gap-2 mb-1">
              <h3 class="text-xl font-semibold text-white">PHP Developer</h3>
              <span class="text-sm text-blue-400 font-mono">2022 — Present</span>
            </div>
            <p class="text-blue-400/80 text-sm font-medium mb-3">Lognet</p>
            <p class="text-gray-400 text-sm leading-relaxed">
              Working on a large logistics web application, maintaining and extending a legacy PHP/MVC/OOP codebase while developing new functionality based on business requirements.
            </p>

            <div class="mt-5">
              <h4 class="text-sm font-semibold text-gray-300 uppercase tracking-wider mb-3">Key Responsibilities</h4>
              <ul class="grid sm:grid-cols-2 gap-x-6 gap-y-1.5 text-sm text-gray-300 list-disc list-inside">
                <li>Maintaining and extending a large legacy PHP/MVC/OOP logistics application</li>
                <li>Developing new functionality based on business requirements</li>
                <li>Working with PHP, MySQL and JavaScript</li>
                <li>Debugging and resolving issues in an existing large codebase</li>
                <li>Working with complex business logic</li>
                <li>Working with existing database structures and SQL queries</li>
                <li>Maintaining existing functionality and improving the application</li>
                <li>Investigating and resolving production issues</li>
              </ul>
            </div>

            <div class="mt-5 pt-5 border-t border-gray-800/50">
              <h4 class="text-sm font-semibold text-gray-300 uppercase tracking-wider mb-3">Technologies</h4>
              <div class="flex flex-wrap gap-2">
                <span class="px-3 py-1 bg-blue-900/30 text-blue-300 text-xs font-mono rounded-full border border-blue-800/50">PHP</span>
                <span class="px-3 py-1 bg-blue-900/30 text-blue-300 text-xs font-mono rounded-full border border-blue-800/50">MySQL</span>
                <span class="px-3 py-1 bg-blue-900/30 text-blue-300 text-xs font-mono rounded-full border border-blue-800/50">JavaScript</span>
                <span class="px-3 py-1 bg-blue-900/30 text-blue-300 text-xs font-mono rounded-full border border-blue-800/50">Git</span>
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- Projects Section -->
      <section id="projects" class="py-16 md:py-24 px-4 sm:px-6 lg:px-8 max-w-6xl mx-auto border-t border-gray-800/50">
        <h2 class="text-3xl md:text-4xl font-bold text-white mb-4">Featured Projects</h2>
        <div class="grid md:grid-cols-2 lg:grid-cols-3 gap-6 mt-8">
          <!-- Project 1 -->
          <div class="bg-[#111827] border border-gray-800 rounded-md overflow-hidden flex flex-col">
            <div class="h-40 bg-gradient-to-br from-blue-900/30 to-gray-900 border-b border-gray-800 flex items-center justify-center">
              <span class="text-3xl font-mono text-blue-400/60">X</span>
            </div>
            <div class="p-5 flex-1 flex flex-col">
              <h3 class="text-lg font-semibold text-white">X Clone</h3>
              <p class="text-gray-400 text-sm mt-1 flex-1">A social-media-style web application built with Laravel and Vue 3, focused on creating, managing and displaying posts and related content.</p>
              <div class="flex flex-wrap gap-1.5 mt-3">
                <span class="px-2 py-0.5 bg-blue-900/30 text-blue-300 text-xs font-mono rounded">Laravel</span>
                <span class="px-2 py-0.5 bg-blue-900/30 text-blue-300 text-xs font-mono rounded">PHP</span>
                <span class="px-2 py-0.5 bg-blue-900/30 text-blue-300 text-xs font-mono rounded">Vue 3</span>
                <span class="px-2 py-0.5 bg-blue-900/30 text-blue-300 text-xs font-mono rounded">Inertia.js</span>
                <span class="px-2 py-0.5 bg-blue-900/30 text-blue-300 text-xs font-mono rounded">MySQL</span>
                <span class="px-2 py-0.5 bg-blue-900/30 text-blue-300 text-xs font-mono rounded">Tailwind</span>
                <span class="px-2 py-0.5 bg-blue-900/30 text-blue-300 text-xs font-mono rounded">Docker</span>
              </div>
              <a href="https://github.com/MaximovicDj/x-clone" target="_blank" rel="noopener noreferrer" class="mt-4 inline-flex items-center gap-2 text-sm text-blue-400 hover:text-blue-300 transition-colors duration-200">
                <svg class="w-4 h-4" fill="currentColor" viewBox="0 0 24 24">
                  <path d="M12 0C5.37 0 0 5.37 0 12c0 5.31 3.435 9.795 8.205 11.385.6.105.825-.255.825-.57 0-.285-.015-1.23-.015-2.235-3.015.555-3.795-.735-4.035-1.41-.135-.345-.72-1.41-1.23-1.695-.42-.225-1.02-.78-.015-.795.945-.015 1.62.87 1.845 1.23 1.08 1.815 2.805 1.305 3.495.99.105-.78.42-1.305.765-1.605-2.67-.3-5.46-1.335-5.46-5.925 0-1.305.465-2.385 1.23-3.225-.12-.3-.54-1.53.12-3.15 0 0 1.005-.315 3.3 1.23.96-.27 1.98-.405 3-.405s2.04.135 3 .405c2.295-1.56 3.3-1.23 3.3-1.23.66 1.62.24 2.85.12 3.15.765.84 1.23 1.905 1.23 3.225 0 4.605-2.805 5.625-5.475 5.925.435.375.81 1.095.81 2.22 0 1.605-.015 2.895-.015 3.3 0 .315.225.69.825.57A12.02 12.02 0 0024 12c0-6.63-5.37-12-12-12z" />
                </svg>
                View on GitHub
              </a>
            </div>
          </div>

          <!-- Project 2 -->
          <div class="bg-[#111827] border border-gray-800 rounded-md overflow-hidden flex flex-col">
            <div class="h-40 bg-gradient-to-br from-blue-900/30 to-gray-900 border-b border-gray-800 flex items-center justify-center">
              <span class="text-2xl font-mono text-blue-400/60">REST API</span>
            </div>
            <div class="p-5 flex-1 flex flex-col">
              <h3 class="text-lg font-semibold text-white">Blog REST API</h3>
              <p class="text-gray-400 text-sm mt-1 flex-1">A REST API built with Laravel for managing blog posts and related resources.</p>
              <div class="flex flex-wrap gap-1.5 mt-3">
                <span class="px-2 py-0.5 bg-blue-900/30 text-blue-300 text-xs font-mono rounded">Laravel</span>
                <span class="px-2 py-0.5 bg-blue-900/30 text-blue-300 text-xs font-mono rounded">PHP</span>
                <span class="px-2 py-0.5 bg-blue-900/30 text-blue-300 text-xs font-mono rounded">REST API</span>
                <span class="px-2 py-0.5 bg-blue-900/30 text-blue-300 text-xs font-mono rounded">MySQL</span>
                <span class="px-2 py-0.5 bg-blue-900/30 text-blue-300 text-xs font-mono rounded">Eloquent</span>
                <span class="px-2 py-0.5 bg-blue-900/30 text-blue-300 text-xs font-mono rounded">Validation</span>
              </div>
              <a href="https://github.com/MaximovicDj/Api-Blog-Post" target="_blank" rel="noopener noreferrer" class="mt-4 inline-flex items-center gap-2 text-sm text-blue-400 hover:text-blue-300 transition-colors duration-200">
                <svg class="w-4 h-4" fill="currentColor" viewBox="0 0 24 24">
                  <path d="M12 0C5.37 0 0 5.37 0 12c0 5.31 3.435 9.795 8.205 11.385.6.105.825-.255.825-.57 0-.285-.015-1.23-.015-2.235-3.015.555-3.795-.735-4.035-1.41-.135-.345-.72-1.41-1.23-1.695-.42-.225-1.02-.78-.015-.795.945-.015 1.62.87 1.845 1.23 1.08 1.815 2.805 1.305 3.495.99.105-.78.42-1.305.765-1.605-2.67-.3-5.46-1.335-5.46-5.925 0-1.305.465-2.385 1.23-3.225-.12-.3-.54-1.53.12-3.15 0 0 1.005-.315 3.3 1.23.96-.27 1.98-.405 3-.405s2.04.135 3 .405c2.295-1.56 3.3-1.23 3.3-1.23.66 1.62.24 2.85.12 3.15.765.84 1.23 1.905 1.23 3.225 0 4.605-2.805 5.625-5.475 5.925.435.375.81 1.095.81 2.22 0 1.605-.015 2.895-.015 3.3 0 .315.225.69.825.57A12.02 12.02 0 0024 12c0-6.63-5.37-12-12-12z" />
                </svg>
                View on GitHub
              </a>
            </div>
          </div>

          <!-- Project 3 -->
          <div class="bg-[#111827] border border-gray-800 rounded-md overflow-hidden flex flex-col">
            <div class="h-40 bg-gradient-to-br from-blue-900/30 to-gray-900 border-b border-gray-800 flex items-center justify-center">
              <span class="text-2xl font-mono text-blue-400/60">Contact App</span>
            </div>
            <div class="p-5 flex-1 flex flex-col">
              <h3 class="text-lg font-semibold text-white">Contact App</h3>
              <p class="text-gray-400 text-sm mt-1 flex-1">A simple contact management application built with Laravel and Blade, featuring contact creation, editing, deletion, search and filtering functionality.</p>
              <div class="flex flex-wrap gap-1.5 mt-3">
                <span class="px-2 py-0.5 bg-blue-900/30 text-blue-300 text-xs font-mono rounded">Laravel</span>
                <span class="px-2 py-0.5 bg-blue-900/30 text-blue-300 text-xs font-mono rounded">PHP</span>
                <span class="px-2 py-0.5 bg-blue-900/30 text-blue-300 text-xs font-mono rounded">Blade</span>
                <span class="px-2 py-0.5 bg-blue-900/30 text-blue-300 text-xs font-mono rounded">Eloquent ORM</span>
                <span class="px-2 py-0.5 bg-blue-900/30 text-blue-300 text-xs font-mono rounded">CRUD</span>
                <span class="px-2 py-0.5 bg-blue-900/30 text-blue-300 text-xs font-mono rounded">Search</span>
                <span class="px-2 py-0.5 bg-blue-900/30 text-blue-300 text-xs font-mono rounded">Filters</span>
                <span class="px-2 py-0.5 bg-blue-900/30 text-blue-300 text-xs font-mono rounded">Validation</span>
                <span class="px-2 py-0.5 bg-blue-900/30 text-blue-300 text-xs font-mono rounded">Form Handling</span>
              </div>
              <a href="https://github.com/MaximovicDj/Laravel-Contact-App" target="_blank" rel="noopener noreferrer" class="mt-4 inline-flex items-center gap-2 text-sm text-blue-400 hover:text-blue-300 transition-colors duration-200">
                <svg class="w-4 h-4" fill="currentColor" viewBox="0 0 24 24">
                  <path d="M12 0C5.37 0 0 5.37 0 12c0 5.31 3.435 9.795 8.205 11.385.6.105.825-.255.825-.57 0-.285-.015-1.23-.015-2.235-3.015.555-3.795-.735-4.035-1.41-.135-.345-.72-1.41-1.23-1.695-.42-.225-1.02-.78-.015-.795.945-.015 1.62.87 1.845 1.23 1.08 1.815 2.805 1.305 3.495.99.105-.78.42-1.305.765-1.605-2.67-.3-5.46-1.335-5.46-5.925 0-1.305.465-2.385 1.23-3.225-.12-.3-.54-1.53.12-3.15 0 0 1.005-.315 3.3 1.23.96-.27 1.98-.405 3-.405s2.04.135 3 .405c2.295-1.56 3.3-1.23 3.3-1.23.66 1.62.24 2.85.12 3.15.765.84 1.23 1.905 1.23 3.225 0 4.605-2.805 5.625-5.475 5.925.435.375.81 1.095.81 2.22 0 1.605-.015 2.895-.015 3.3 0 .315.225.69.825.57A12.02 12.02 0 0024 12c0-6.63-5.37-12-12-12z" />
                </svg>
                View on GitHub
              </a>
            </div>
          </div>
        </div>
      </section>

      <!-- Contact Section -->
      <section id="contact" class="py-16 md:py-24 px-4 sm:px-6 lg:px-8 max-w-6xl mx-auto border-t border-gray-800/50">
        <h2 class="text-3xl md:text-4xl font-bold text-white mb-2">Let's Work Together</h2>
        <p class="text-gray-400 max-w-xl">If you have a project, opportunity or just want to get in touch, feel free to send me a message.</p>
        <div class="grid md:grid-cols-3 gap-8 mt-8">
          <!-- Contact Form -->
          <form
              @submit.prevent="submitForm"
              action="https://formspree.io/f/mgaeeowv"
              method="POST"
              class="md:col-span-2 space-y-4">
            <div>
              <label for="name" class="block text-sm font-medium text-gray-300 mb-1">Name</label>
              <input type="text" name="name" id="name" class="w-full px-4 py-2 bg-[#111827] border border-gray-700 rounded-md focus:outline-none focus:border-blue-500 focus:ring-1 focus:ring-blue-500 text-white placeholder-gray-500 transition-colors duration-200" placeholder="Your name">
            </div>
            <div>
              <label for="email" class="block text-sm font-medium text-gray-300 mb-1">Email</label>
              <input type="email" name="email" id="email" class="w-full px-4 py-2 bg-[#111827] border border-gray-700 rounded-md focus:outline-none focus:border-blue-500 focus:ring-1 focus:ring-blue-500 text-white placeholder-gray-500 transition-colors duration-200" placeholder="your@email.com">
            </div>
            <div>
              <label for="subject" class="block text-sm font-medium text-gray-300 mb-1">Subject</label>
              <input type="text" name="subject" id="subject" class="w-full px-4 py-2 bg-[#111827] border border-gray-700 rounded-md focus:outline-none focus:border-blue-500 focus:ring-1 focus:ring-blue-500 text-white placeholder-gray-500 transition-colors duration-200" placeholder="Subject">
            </div>
            <div>
              <label for="message" class="block text-sm font-medium text-gray-300 mb-1">Message</label>
              <textarea name="message" id="message" rows="5" class="w-full px-4 py-2 bg-[#111827] border border-gray-700 rounded-md focus:outline-none focus:border-blue-500 focus:ring-1 focus:ring-blue-500 text-white placeholder-gray-500 transition-colors duration-200 resize-none" placeholder="Your message..."></textarea>
            </div>
            <button type="submit" class="px-6 py-3 bg-blue-600 hover:bg-blue-700 text-white font-medium rounded-md transition-colors duration-200 w-full sm:w-auto">
              Send Message
            </button>
          </form>

          <!-- Contact Info -->
          <div class="space-y-6">
            <div>
              <h3 class="text-sm font-semibold text-gray-400 uppercase tracking-wider">Email</h3>
              <a href="mailto:maximovic1996@gmail.com" class="text-gray-200 hover:text-blue-400 transition-colors duration-200">maximovic1996@gmail.com</a>
            </div>
            <div>
              <h3 class="text-sm font-semibold text-gray-400 uppercase tracking-wider">GitHub</h3>
              <a href="https://github.com/MaximovicDj" target="_blank" rel="noopener noreferrer" class="text-gray-200 hover:text-blue-400 transition-colors duration-200">MaximovicDj</a>
            </div>
          </div>
        </div>
      </section>
    </main>

    <!-- Footer -->
    <footer class="border-t border-gray-800/50 py-8 px-4 sm:px-6 lg:px-8 max-w-6xl mx-auto">
      <div class="flex flex-col sm:flex-row justify-between items-center gap-4 text-sm text-gray-400">
        <div class="text-center sm:text-left">
          <p class="text-white font-medium">Djordje Maksimovic</p>
          <p class="text-xs text-blue-400">PHP / Laravel Developer</p>
        </div>
        <div class="flex gap-6">
          <a href="https://github.com/MaximovicDj" target="_blank" rel="noopener noreferrer" class="hover:text-blue-400 transition-colors duration-200" aria-label="GitHub">
            <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 24 24">
              <path d="M12 0C5.37 0 0 5.37 0 12c0 5.31 3.435 9.795 8.205 11.385.6.105.825-.255.825-.57 0-.285-.015-1.23-.015-2.235-3.015.555-3.795-.735-4.035-1.41-.135-.345-.72-1.41-1.23-1.695-.42-.225-1.02-.78-.015-.795.945-.015 1.62.87 1.845 1.23 1.08 1.815 2.805 1.305 3.495.99.105-.78.42-1.305.765-1.605-2.67-.3-5.46-1.335-5.46-5.925 0-1.305.465-2.385 1.23-3.225-.12-.3-.54-1.53.12-3.15 0 0 1.005-.315 3.3 1.23.96-.27 1.98-.405 3-.405s2.04.135 3 .405c2.295-1.56 3.3-1.23 3.3-1.23.66 1.62.24 2.85.12 3.15.765.84 1.23 1.905 1.23 3.225 0 4.605-2.805 5.625-5.475 5.925.435.375.81 1.095.81 2.22 0 1.605-.015 2.895-.015 3.3 0 .315.225.69.825.57A12.02 12.02 0 0024 12c0-6.63-5.37-12-12-12z" />
            </svg>
          </a>
          <a href="https://www.linkedin.com/in/djordje-maksimovic-907b39217/" target="_blank" rel="noopener noreferrer" class="hover:text-blue-400 transition-colors duration-200" aria-label="LinkedIn">
            <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 24 24">
              <path d="M20.447 20.452h-3.554v-5.569c0-1.328-.027-3.037-1.852-3.037-1.853 0-2.136 1.445-2.136 2.939v5.667H9.351V9h3.414v1.561h.046c.477-.9 1.637-1.85 3.37-1.85 3.601 0 4.267 2.37 4.267 5.455v6.286zM5.337 7.433a2.062 2.062 0 01-2.063-2.065 2.064 2.064 0 112.063 2.065zm1.782 13.019H3.555V9h3.564v11.452zM22.225 0H1.771C.792 0 0 .774 0 1.729v20.542C0 23.227.792 24 1.771 24h20.451C23.2 24 24 23.227 24 22.271V1.729C24 .774 23.2 0 22.222 0h.003z" />
            </svg>
          </a>
          <a href="mailto:maximovic1996@gmail.com" class="hover:text-blue-400 transition-colors duration-200" aria-label="Email">
            <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 8l7.89 5.26a2 2 0 002.22 0L21 8M5 19h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z" />
            </svg>
          </a>
        </div>
        <p class="text-xs text-gray-500">&copy; {{ currentYear }} Djordje Maksimovic. All rights reserved.</p>
      </div>
    </footer>
  </div>
</template>