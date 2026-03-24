<template>
  <div class="site">
    <header class="topbar">
      <div class="container topbar__inner">
        <div class="nav-wrap">
          <nav class="nav" aria-label="Primary">
          <a
            class="nav__link"
            :class="{ 'is-active': activeSection === 'home' }"
            href="#home"
            @click.prevent="scrollTo('home')"
            >Home</a
          >
          <a
            class="nav__link"
            :class="{ 'is-active': activeSection === 'about' }"
            href="#about"
            @click.prevent="scrollTo('about')"
            >About</a
          >
          <span class="nav__badge" aria-hidden="true">{{ initials }}</span>
          <a
            class="nav__link"
            :class="{ 'is-active': activeSection === 'projects' }"
            href="#projects"
            @click.prevent="scrollTo('projects')"
            >Portfolio</a
          >
          <a
            class="nav__link"
            :class="{ 'is-active': activeSection === 'contact' }"
            href="#contact"
            @click.prevent="scrollTo('contact')"
            >Contact</a
          >
          </nav>
        </div>

        <button class="theme-toggle" type="button" @click="toggleTheme" aria-label="Toggle theme">
          <span v-if="theme === 'dark'">🌙</span>
          <span v-else>☀️</span>
        </button>
      </div>
    </header>

    <main>
      <section id="home" class="hero">
        <div class="container hero__grid">
          <div class="hero-left">
            <div class="kicker">
              <span class="kicker__pill" />
              <span>{{ role }}</span>
            </div>

            <h1 class="hero__title">
              I'm <strong>{{ name }}</strong>,
              <br />
              {{ role }}
            </h1>

            <p class="hero__subtitle">
              {{ tagline }}
            </p>

            <div class="cta-row">
              <a class="btn btn--cta" href="#about" @click.prevent="scrollTo('about')">About</a>
              <a class="btn btn--cta" v-if="cvUrl" :href="cvUrl" download>Download CV</a>
              <a class="btn btn--cta" href="#contact" @click.prevent="scrollTo('contact')">Contact Me</a>
              <a
                class="btn btn--cta"
                :href="social.github"
                target="_blank"
                rel="noopener noreferrer"
                v-if="social.github"
              >
                GitHub
              </a>
            </div>
          </div>

          <aside class="profile-card" aria-label="Profile">
            <div class="profile-card__top">
              <button
                type="button"
                class="photo-frame"
                @click="openProfileModal"
                :disabled="!profileImage"
                aria-label="Open profile photo"
              >
                <img v-if="profileImage" :src="profileImage" class="avatar-img" alt="Profile photo" />
              </button>
              <div class="profile-meta">
                <p class="profile-card__name">{{ name }}</p>
                <p class="profile-card__role">{{ role }}</p>
              </div>
            </div>

            <div class="facts">
              <div class="fact">
                <div class="fact__label">Location</div>
                <div class="fact__value">{{ location }}</div>
              </div>
              <div class="fact">
                <div class="fact__label">Email</div>
                <div class="fact__value">{{ email }}</div>
              </div>
              <div class="fact">
                <div class="fact__label">Phone</div>
                <div class="fact__value">{{ phone || '—' }}</div>
              </div>
            </div>
          </aside>
        </div>
      </section>

      <div v-if="showProfileModal" class="modal-overlay" role="dialog" aria-modal="true" @click.self="closeProfileModal">
        <div class="modal-card">
          <button type="button" class="modal-close" @click="closeProfileModal" aria-label="Close profile photo">
            &times;
          </button>
          <img v-if="profileImage" class="modal-img" :src="profileImage" alt="Profile photo enlarged" />
        </div>
      </div>

      <section id="about" class="section">
        <div class="container">
          <div class="section__header">
            <h2 class="section__title">About</h2>
            <p class="section__desc">A quick summary of what I do and how I work.</p>
          </div>

          <div class="grid-2">
            <div class="card">
              <h3>My Story</h3>
              <p class="muted" style="margin: 0; white-space: pre-line">{{ about }}</p>
            </div>

            <div class="card">
              <h3>Skills</h3>
              <div style="display: grid; gap: 10px">
                <div>
                  <div class="muted" style="font-size: 13px; margin-bottom: 8px">Technical Skills</div>
                  <div class="tags" v-if="technicalSkills?.length">
                    <span class="tag" v-for="s in technicalSkills" :key="s">{{ s }}</span>
                  </div>
                </div>

                <div>
                  <div class="muted" style="font-size: 13px; margin-bottom: 8px">Soft Skills</div>
                  <div class="tags" v-if="softSkills?.length">
                    <span class="tag" v-for="s in softSkills" :key="s">{{ s }}</span>
                  </div>
                </div>

                <div>
                  <div class="muted" style="font-size: 13px; margin-bottom: 8px">Languages</div>
                  <div class="tags" v-if="languages?.length">
                    <span class="tag" v-for="s in languages" :key="s">{{ s }}</span>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </section>

      <section id="projects" class="section">
        <div class="container">
          <div class="section__header">
            <h2 class="section__title">Portfolio</h2>
            <p class="section__desc">A few things I’ve built.</p>
          </div>

          <div class="projects-grid">
            <article class="card project-card" v-for="p in projects" :key="p.title">
              <img v-if="p.image" class="project-thumb" :src="p.image" :alt="p.title" />
              <div class="project-card__title">
                <h3>{{ p.title }}</h3>
                <span class="badge" v-if="p.badge">{{ p.badge }}</span>
              </div>

              <p class="muted" style="margin: 10px 0 0">{{ p.description }}</p>

              <div class="tags" v-if="p.tags?.length">
                <span class="tag" v-for="t in p.tags" :key="t">{{ t }}</span>
              </div>

              <div class="project-actions">
                <a v-if="p.url" class="btn" :href="p.url" target="_blank" rel="noopener noreferrer">View</a>
                <button class="btn" type="button" @click="copyLink(p.url)" v-if="p.url">
                  Copy Link
                </button>
              </div>
            </article>
          </div>
        </div>
      </section>

      <section id="education" class="section">
        <div class="container">
          <div class="section__header">
            <h2 class="section__title">Education</h2>
          </div>

          <div class="grid-2">
            <div class="card" style="grid-column: 1 / -1">
              <div style="display: grid; gap: 12px">
                <div v-for="e in education" :key="e.school" class="fact">
                  <div>
                    <div style="font-weight: 700; margin-bottom: 2px">{{ e.school }}</div>
                    <div class="fact__label" style="margin-top: 4px">{{ e.from }} - {{ e.to }}</div>
                  </div>
                  <div class="fact__value"></div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </section>

      <section id="contact" class="section">
        <div class="container">
          <div class="section__header">
            <h2 class="section__title">Contact</h2>
          </div>

          <div class="card">
            <form class="form" @submit.prevent="submitContact">
              <div class="field field--full" style="margin-bottom: 2px">
                <div class="muted" style="font-size: 13px">
                  Email: <a :href="`mailto:${email}`" style="text-decoration: underline">{{ email }}</a>
                  &nbsp;|&nbsp;
                  Phone: <a :href="`tel:${phone}`" style="text-decoration: underline">{{ phone }}</a>
                </div>
                <div class="muted" style="font-size: 13px; margin-top: 6px">Location: {{ address }}</div>
              </div>

              <div class="field">
                <label for="name">Name</label>
                <input id="name" v-model="form.name" type="text" required maxlength="120" />
              </div>

              <div class="field">
                <label for="email">Email</label>
                <input id="email" v-model="form.email" type="email" required maxlength="255" />
              </div>

              <div class="field field--full">
                <label for="message">Message</label>
                <textarea id="message" v-model="form.message" required maxlength="2000" />
              </div>

              <div class="form__actions">
                <div class="status" v-if="submitError">{{ submitError }}</div>
                <div class="status" v-else-if="submitSuccess">Thanks! I’ll get back to you soon.</div>

                <button class="btn btn--primary" type="submit" :disabled="submitting">
                  {{ submitting ? 'Sending...' : 'Send Message' }}
                </button>
              </div>
            </form>
          </div>
        </div>
      </section>
    </main>

    <footer>
      <div class="container">
        <div class="muted" style="font-size: 13px">
          © {{ new Date().getFullYear() }} {{ name }}. Built with Laravel + Vue.
        </div>
      </div>
    </footer>
  </div>
</template>

<script setup>
import { computed, onMounted, reactive, ref } from 'vue';

import { portfolioData as portfolio } from './data';
const contactUrl = '/contact';

const name = computed(() => portfolio.name || 'Your Name');
const role = computed(() => portfolio.role || 'Web Developer');
const tagline = computed(() => portfolio.tagline || 'Laravel + Vue with a focus on UI polish.');
const location = computed(() => portfolio.location || 'Your City');
const email = computed(() => portfolio.email || 'you@example.com');
const phone = computed(() => portfolio.phone || '');
const address = computed(() => portfolio.address || location.value);
const about = computed(() => portfolio.about || 'Write a short bio here.');

const technicalSkills = computed(() => portfolio.technicalSkills || []);
const softSkills = computed(() => portfolio.softSkills || []);
const languages = computed(() => portfolio.languages || []);
const projects = computed(() => portfolio.projects || []);
const education = computed(() => portfolio.education || []);
const profileImage = computed(() => portfolio.profileImage || '');
const cvUrl = computed(() => portfolio.cvUrl || '');

const social = computed(() => ({
  github: portfolio.social?.github || '',
  linkedin: portfolio.social?.linkedin || '',
}));

const initials = computed(() => {
  const parts = String(name.value).split(' ').filter(Boolean);
  if (parts.length >= 2) return `${parts[0][0]}${parts[1][0]}`.toUpperCase();
  return String(name.value).slice(0, 2).toUpperCase();
});

// const axios = window.axios; // Removed, using dummy submission

const form = reactive({
  name: '',
  email: '',
  message: '',
});

const submitting = ref(false);
const submitError = ref('');
const submitSuccess = ref(false);

const theme = ref('dark');
const activeSection = ref('home');
const showProfileModal = ref(false);

function applyTheme(next) {
  theme.value = next;
  document.documentElement.dataset.theme = next;
  try {
    localStorage.setItem('portfolio_theme', next);
  } catch {
    // ignore
  }
}

function toggleTheme() {
  applyTheme(theme.value === 'dark' ? 'light' : 'dark');
}

function openProfileModal() {
  if (!profileImage.value) return;
  showProfileModal.value = true;
}

function closeProfileModal() {
  showProfileModal.value = false;
}

function scrollTo(id) {
  const el = document.getElementById(id);
  if (!el) return;
  activeSection.value = id;
  el.scrollIntoView({ behavior: 'smooth', block: 'start' });
}

function copyLink(url) {
  if (!url) return;
  navigator.clipboard?.writeText(url).catch(() => {});
}

async function submitContact() {
  submitError.value = '';
  submitSuccess.value = false;
  submitting.value = true;

  try {
    // Dummy submission delay since there is no backend
    await new Promise(resolve => setTimeout(resolve, 1000));

    submitting.value = false;
    submitSuccess.value = true;
    form.name = '';
    form.email = '';
    form.message = '';
  } catch (e) {
    submitting.value = false;
    submitError.value = 'Something went wrong. Please try again.';
  }
}

onMounted(() => {
  const saved = (() => {
    try {
      return localStorage.getItem('portfolio_theme');
    } catch {
      return null;
    }
  })();

  const prefersDark = typeof window !== 'undefined' && window.matchMedia?.('(prefers-color-scheme: dark)')?.matches;
  applyTheme(saved === 'light' || saved === 'dark' ? saved : prefersDark ? 'dark' : 'light');

  // Basic active section highlighting for the nav pill.
  const sections = Array.from(document.querySelectorAll('section[id]'));
  const onScroll = () => {
    const anchor = 120;
    let current = 'home';
    for (const s of sections) {
      const top = s.getBoundingClientRect().top;
      if (top <= anchor) current = s.id;
    }
    activeSection.value = current;
  };

  window.addEventListener('scroll', onScroll, { passive: true });
  onScroll();

  window.addEventListener('keydown', (e) => {
    if (e.key === 'Escape') closeProfileModal();
  });
});
</script>

