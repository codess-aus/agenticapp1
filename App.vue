<template>
  <div class="app-container" :class="{'dark-theme': darkMode}">
    <header v-if="onboardingCompleted">
      <div class="logo-container">
        <img src="./assets/GHCPAgent1.jpg" alt="App logo" class="logo">
        <h1>User Management System</h1>
      </div>
      <div class="header-actions">
        <button @click="darkMode = !darkMode" class="icon-button">
          <svg v-if="darkMode" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <circle cx="12" cy="12" r="5"></circle>
            <line x1="12" y1="1" x2="12" y2="3"></line>
            <line x1="12" y1="21" x2="12" y2="23"></line>
            <line x1="4.22" y1="4.22" x2="5.64" y2="5.64"></line>
            <line x1="18.36" y1="18.36" x2="19.78" y2="19.78"></line>
            <line x1="1" y1="12" x2="3" y2="12"></line>
            <line x1="21" y1="12" x2="23" y2="12"></line>
            <line x1="4.22" y1="19.78" x2="5.64" y2="18.36"></line>
            <line x1="18.36" y1="5.64" x2="19.78" y2="4.22"></line>
          </svg>
          <svg v-else xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <path d="M21 12.79A9 9 0 1 1 11.21 3 7 7 0 0 0 21 12.79z"></path>
          </svg>
        </button>
        <button class="user-profile">
          {{ username.charAt(0).toUpperCase() }}
        </button>
      </div>
    </header>

    <main>
      <transition name="fade" mode="out-in">
        <Onboarding v-if="!onboardingCompleted" @onboarding-completed="completeOnboarding" />
        <div v-else class="dashboard">
          <h2>Welcome, {{ username }}!</h2>
          <p>Your account has been set up successfully.</p>
          <div class="dashboard-card">
            <h3>What's next?</h3>
            <ul class="feature-list">
              <li>
                <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                  <path d="M16 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"></path>
                  <circle cx="8.5" cy="7" r="4"></circle>
                  <line x1="20" y1="8" x2="20" y2="14"></line>
                  <line x1="23" y1="11" x2="17" y2="11"></line>
                </svg>
                Invite team members
              </li>
              <li>
                <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                  <rect x="3" y="4" width="18" height="18" rx="2" ry="2"></rect>
                  <line x1="16" y1="2" x2="16" y2="6"></line>
                  <line x1="8" y1="2" x2="8" y2="6"></line>
                  <line x1="3" y1="10" x2="21" y2="10"></line>
                </svg>
                Set up your calendar
              </li>
              <li>
                <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                  <path d="M21 16V8a2 2 0 0 0-1-1.73l-7-4a2 2 0 0 0-2 0l-7 4A2 2 0 0 0 3 8v8a2 2 0 0 0 1 1.73l7 4a2 2 0 0 0 2 0l7-4A2 2 0 0 0 21 16z"></path>
                  <polyline points="3.27 6.96 12 12.01 20.73 6.96"></polyline>
                  <line x1="12" y1="22.08" x2="12" y2="12"></line>
                </svg>
                Explore the dashboard
              </li>
            </ul>
          </div>
        </div>
      </transition>
    </main>
  </div>
</template>

<script>
import Onboarding from './components/Onboarding.vue'
import UserForm from './components/UserForm.vue'

export default {
  components: {
    Onboarding,
    UserForm
  },
  data() {
    return {
      onboardingCompleted: false,
      darkMode: false,
      username: 'User',
      userPreferences: {}
    }
  },
  methods: {
    completeOnboarding(data) {
      this.onboardingCompleted = true;
      this.userPreferences = data.preferences;
      
      if (this.userPreferences.theme === 'dark') {
        this.darkMode = true;
      } else if (this.userPreferences.theme === 'auto') {
        // Check system preferences
        this.darkMode = window.matchMedia('(prefers-color-scheme: dark)').matches;
      }
      
      // For demo purposes, we'll set a username
      this.username = 'Demo User';
    }
  }
}
</script>

<style>
:root {
  --primary-color: #42b883;
  --primary-light: rgba(66, 184, 131, 0.1);
  --primary-dark: #3aa876;
  --text-color: #2c3e50;
  --secondary-text: #666;
  --bg-color: #fff;
  --card-bg: #fff;
  --border-color: #eaeaea;
  --shadow-color: rgba(0, 0, 0, 0.05);
  --secondary-bg: #f9f9f9;
  --success-color: #4CAF50;
  --success-bg: rgba(76, 175, 80, 0.1);
  --transition-speed: 0.3s;
}

.dark-theme {
  --text-color: #e1e1e1;
  --secondary-text: #b0b0b0;
  --bg-color: #121212;
  --card-bg: #1e1e1e;
  --border-color: #333;
  --shadow-color: rgba(0, 0, 0, 0.2);
  --secondary-bg: #262626;
}

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: Inter, Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  background-color: var(--bg-color);
  color: var(--text-color);
  transition: background-color var(--transition-speed), color var(--transition-speed);
}

.app-container {
  min-height: 100vh;
  padding: 0 0 40px 0;
  transition: background-color var(--transition-speed), color var(--transition-speed);
  background-color: var(--bg-color);
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px 40px;
  box-shadow: 0 2px 10px var(--shadow-color);
  background-color: var(--card-bg);
}

.logo-container {
  display: flex;
  align-items: center;
}

.logo {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  margin-right: 15px;
}

h1 {
  font-size: 1.5rem;
  font-weight: 600;
}

main {
  padding: 40px;
  max-width: 1200px;
  margin: 0 auto;
}

.header-actions {
  display: flex;
  align-items: center;
}

.icon-button {
  background: none;
  border: none;
  color: var(--secondary-text);
  cursor: pointer;
  padding: 8px;
  border-radius: 50%;
  margin-right: 15px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s;
}

.icon-button:hover {
  background-color: var(--secondary-bg);
  color: var(--primary-color);
}

.user-profile {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  background-color: var(--primary-color);
  color: white;
  border: none;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: bold;
  font-size: 18px;
  transition: all 0.2s;
}

.user-profile:hover {
  transform: scale(1.05);
  box-shadow: 0 2px 10px rgba(66, 184, 131, 0.3);
}

.dashboard {
  text-align: center;
}

.dashboard h2 {
  margin-bottom: 10px;
  color: var(--primary-color);
}

.dashboard-card {
  background-color: var(--card-bg);
  border-radius: 12px;
  padding: 30px;
  margin-top: 40px;
  box-shadow: 0 8px 30px var(--shadow-color);
  max-width: 600px;
  margin-left: auto;
  margin-right: auto;
  transition: background-color var(--transition-speed), box-shadow var(--transition-speed);
}

.feature-list {
  list-style: none;
  margin-top: 20px;
  text-align: left;
}

.feature-list li {
  display: flex;
  align-items: center;
  padding: 15px 0;
  border-bottom: 1px solid var(--border-color);
}

.feature-list li:last-child {
  border-bottom: none;
}

.feature-list li svg {
  margin-right: 15px;
  color: var(--primary-color);
}

/* Transitions */
.fade-enter-active,
.fade-leave-active {
  transition: opacity var(--transition-speed), transform var(--transition-speed);
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
  transform: translateY(20px);
}

@media (max-width: 768px) {
  header {
    padding: 15px 20px;
  }
  
  main {
    padding: 20px;
  }
  
  h1 {
    font-size: 1.2rem;
  }
}
</style>