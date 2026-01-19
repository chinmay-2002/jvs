<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'

const slides = [
  {
    image: '/images/jaggery.jpg',
    tag: 'Natural & Pure',
    title: 'Premium Organic Jaggery',
    subtitle: 'Traditionally crafted from fresh sugarcane',
  },
  {
    image: '/images/textile.jpg',
    tag: 'Manufacturing Excellence',
    title: 'High-Quality Textile Solutions',
    subtitle: 'Trusted fabrics for modern businesses',
  },
  {
    image: '/images/heritage.jpg',
    tag: 'Natural & Pure',
    title: 'Premium Organic Jaggery',
    subtitle: 'Traditionally crafted from fresh sugarcane',
  },
]

const current = ref(0)
let timer: number

onMounted(() => {
  timer = window.setInterval(() => {
    current.value = (current.value + 1) % slides.length
  }, 6500)
})

onUnmounted(() => clearInterval(timer))
</script>

<template>
  <section class="hero-banner">
    <div
      v-for="(slide, index) in slides"
      :key="index"
      class="hero-slide"
      :class="{ active: index === current }"
      :style="{ backgroundImage: `url(${slide.image})` }"
    >
      <div class="overlay"></div>

      <div class="container h-100">
        <div class="row h-100 align-items-center justify-content-center text-center">
          <div class="col-lg-8">
            <!-- Logo -->
            <img src="/images/logo.jpg" alt="Company Logo" class="hero-logo mb-4" />

            <!-- Tag -->
            <div class="hero-tag mb-3">
              {{ slide.tag }}
            </div>

            <!-- Title -->
            <h1 class="hero-title mb-3">
              <span class="reveal">{{ slide.title }}</span>
            </h1>

            <!-- Subtitle -->
            <p class="hero-subtitle mb-5">
              {{ slide.subtitle }}
            </p>

            <!-- Buttons -->
            <div class="d-flex justify-content-center gap-4 flex-wrap">
              <router-link to="/contact" class="btn btn-danger btn-lg px-5">
                Contact Us
              </router-link>
              <router-link to="/about" class="btn btn-outline-light ml-3 btn-lg px-5">
                Our Story
              </router-link>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
/* ===== HERO BASE ===== */
.hero-banner {
  position: relative;
  height: 100vh;
  overflow: hidden;
}

/* ===== SLIDES ===== */
.hero-slide {
  position: absolute;
  inset: 0;
  background-size: cover;
  background-position: center;
  opacity: 0;
  transform: scale(1.1);
  transition:
    opacity 1.6s ease,
    transform 1.6s ease;
}

.hero-slide.active {
  opacity: 1;
  transform: scale(1);
  z-index: 1;
}

/* ===== DARK OVERLAY ===== */
.overlay {
  position: absolute;
  inset: 0;
  background: linear-gradient(180deg, rgba(0, 0, 0, 0.7), rgba(0, 0, 0, 0.6));
}

/* ===== LOGO ===== */
.hero-logo {
  width: 96px;
  opacity: 0;
  transform: translateY(10px);
}

.hero-slide.active .hero-logo {
  animation: fadeUp 1s ease forwards;
}

/* ===== TAG ===== */
.hero-tag {
  color: #ef4444;
  letter-spacing: 3px;
  font-size: 0.85rem;
  text-transform: uppercase;
  font-weight: 600;
  opacity: 0;
}

.hero-slide.active .hero-tag {
  animation: fadeUp 1s ease forwards;
  animation-delay: 0.2s;
}

/* ===== TITLE ===== */
.hero-title {
  font-size: clamp(2.8rem, 5vw, 4.5rem);
  font-weight: 800;
  color: #ffffff;
  line-height: 1.15;
  overflow: hidden;
}

.reveal {
  display: inline-block;
  transform: translateY(120%);
}

.hero-slide.active .reveal {
  animation: slideReveal 1s cubic-bezier(0.4, 0, 0.2, 1) forwards;
  animation-delay: 0.4s;
}

/* ===== SUBTITLE ===== */
.hero-subtitle {
  font-size: 1.25rem;
  color: #e5e7eb;
  opacity: 0;
}

.hero-slide.active .hero-subtitle {
  animation: fadeUp 1s ease forwards;
  animation-delay: 0.8s;
}

/* ===== BUTTONS ===== */
.btn-danger {
  background-color: #dc2626;
  border-color: #dc2626;
}

.btn-danger:hover {
  background-color: #b91c1c;
  border-color: #b91c1c;
}

/* ===== ANIMATIONS ===== */
@keyframes slideReveal {
  to {
    transform: translateY(0);
  }
}

@keyframes fadeUp {
  from {
    opacity: 0;
    transform: translateY(24px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

/* ===== MOBILE ===== */
@media (max-width: 768px) {
  .hero-title {
    font-size: 2.4rem;
  }
}
</style>
