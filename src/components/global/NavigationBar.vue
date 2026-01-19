<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'

const isOpen = ref(false)
const isHidden = ref(false)
let lastScroll = 0

const toggleMenu = () => {
  isOpen.value = !isOpen.value
}

const closeMenu = () => {
  isOpen.value = false
}

const handleScroll = () => {
  const current = window.scrollY
  isHidden.value = current > lastScroll && current > 80
  lastScroll = current
}
window.addEventListener('resize', () => {
  if (window.innerWidth > 768) isOpen.value = false
})

onMounted(() => window.addEventListener('scroll', handleScroll))
onUnmounted(() => window.removeEventListener('scroll', handleScroll))
</script>

<template>
  <header :class="['navbar-wrapper', { hidden: isHidden }]">
    <nav class="navbar container">
      <router-link class="brand" to="/"
        ><img src="/images/logo.jpg" alt="Company Logo" class="hero-logo" />
      </router-link>

      <!-- Desktop Menu -->
      <div class="menu">
        <router-link to="/" class="nav-link">Home</router-link>
        <router-link to="/services" class="nav-link">Services</router-link>
        <router-link to="/about" class="nav-link">About</router-link>
        <router-link to="/contact" class="nav-link">Contact</router-link>
      </div>

      <!-- Mobile Toggle -->
      <button
        class="menu-btn"
        @click="toggleMenu"
        aria-label="Toggle navigation"
        :aria-expanded="isOpen"
      >
        <span :class="{ open: isOpen }"></span>
        <span :class="{ open: isOpen }"></span>
        <span :class="{ open: isOpen }"></span>
      </button>
    </nav>

    <!-- Mobile Popup -->
    <transition name="slide">
      <div v-show="isOpen" class="popup-menu">
        <router-link @click="closeMenu" to="/" class="nav-link">Home</router-link>
        <router-link @click="closeMenu" to="/services" class="nav-link">Services</router-link>
        <router-link @click="closeMenu" to="/about" class="nav-link">About</router-link>
        <router-link @click="closeMenu" to="/contact" class="nav-link">Contact</router-link>
      </div>
    </transition>
  </header>
</template>
<style>
/* Wrapper */
.navbar-wrapper {
  position: fixed;
  top: 0;
  width: 100%;
  background: #ffffff;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.08);
  transition: transform 0.3s ease;
  z-index: 1000;
}

.navbar-wrapper.hidden {
  transform: translateY(-100%);
}

/* Navbar */
.navbar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  height: 70px;
}

.container {
  max-width: 1200px;
  margin: auto;
  padding: 0 24px;
}

.brand {
  font-size: 30px;
  color: red;
  font-weight: 700;
  text-decoration: none;
}
.brand:hover {
  color: red;
  text-decoration: none;
}

/* Desktop Menu */
.menu {
  display: flex;
  gap: 28px;
}

.nav-link {
  text-decoration: none;
  color: #374151;
  font-weight: 500;
  position: relative;
}

.nav-link:hover {
  color: #eb2525;
}

.nav-link::after {
  content: '';
  position: absolute;
  bottom: -6px;
  left: 0;
  width: 0;
  height: 2px;
  background: #eb2525;
  transition: width 0.3s;
}

.nav-link:hover::after {
  width: 100%;
}

/* Popup */
.popup-menu {
  background: white;
  padding: 20px;
  display: flex;
  color: #eb2525;
  flex-direction: column;
  gap: 16px;
  border-top: 1px solid #e5e7eb;
}

/* Animations */
.slide-enter-active,
.slide-leave-active {
  transition:
    transform 0.25s ease,
    opacity 0.25s;
}
.slide-enter-from {
  transform: translateY(-10px);
  opacity: 0;
}
.slide-leave-to {
  transform: translateY(-10px);
  opacity: 0;
}

/* Responsive */
@media (max-width: 768px) {
  .menu {
    display: none;
  }
  .menu-btn {
    display: block;
  }
}
.menu-btn {
  width: 30px;
  height: 24px;
  display: none;
  flex-direction: column;
  justify-content: space-between;
  background: none;
  border: none;
  cursor: pointer;
  color: #eb2525;
}

@media (max-width: 768px) {
  .menu-btn {
    display: flex;
  }
}

.menu-btn span {
  height: 3px;
  background: #1f2937;
  border-radius: 4px;
  color: #eb2525;
  transition: all 0.35s ease;
}

/* Animate to X */
.menu-btn span:nth-child(1).open {
  transform: translateY(10px) rotate(45deg);
}

.menu-btn span:nth-child(2).open {
  opacity: 0;
}

.menu-btn span:nth-child(3).open {
  transform: translateY(-10px) rotate(-45deg);
}
.hero-logo {
  width: 50px;
}
</style>
