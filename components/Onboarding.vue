<template>
  <div class="onboarding-container">
    <div class="step-indicators">
      <div 
        v-for="(step, index) in steps" 
        :key="index"
        :class="['step-indicator', { active: currentStep === index, completed: currentStep > index }]"
        @click="goToStep(index)"
      >
        <div class="step-number">{{ index + 1 }}</div>
        <div class="step-label">{{ step.title }}</div>
      </div>
    </div>

    <div class="step-content">
      <transition name="fade" mode="out-in">
        <div v-if="currentStep === 0" key="welcome">
          <h2>Welcome to User Management System</h2>
          <p>Let's set up your account in a few simple steps.</p>
          <img src="../assets/GHCPAgent1.jpg" alt="Welcome" class="welcome-image">
        </div>

        <div v-else-if="currentStep === 1" key="userinfo">
          <h2>Create Your Account</h2>
          <UserForm @user-created="userCreated" />
        </div>

        <div v-else-if="currentStep === 2" key="preferences">
          <h2>Set Your Preferences</h2>
          <div class="preferences-form">
            <div class="form-group">
              <label>Theme</label>
              <div class="theme-options">
                <button 
                  v-for="theme in ['light', 'dark', 'auto']" 
                  :key="theme"
                  :class="['theme-option', { selected: preferences.theme === theme }]"
                  @click="preferences.theme = theme"
                >
                  {{ theme.charAt(0).toUpperCase() + theme.slice(1) }}
                </button>
              </div>
            </div>

            <div class="form-group">
              <label>Email Notifications</label>
              <div class="toggle-switch">
                <input 
                  type="checkbox" 
                  id="notifications" 
                  v-model="preferences.notifications"
                />
                <label for="notifications"></label>
              </div>
            </div>
          </div>
        </div>

        <div v-else-if="currentStep === 3" key="complete">
          <div class="completion-message">
            <svg class="checkmark" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 52 52">
              <circle class="checkmark__circle" cx="26" cy="26" r="25" fill="none"/>
              <path class="checkmark__check" fill="none" d="M14.1 27.2l7.1 7.2 16.7-16.8"/>
            </svg>
            <h2>All Set!</h2>
            <p>Your account has been created successfully.</p>
            <button @click="completeOnboarding" class="primary-button">Get Started</button>
          </div>
        </div>
      </transition>
    </div>

    <div class="step-navigation">
      <button 
        v-if="currentStep > 0" 
        @click="prevStep" 
        class="secondary-button"
      >
        Back
      </button>
      <button 
        v-if="currentStep < steps.length - 1" 
        @click="nextStep" 
        class="primary-button"
        :disabled="currentStep === 1 && !userCreatedFlag"
      >
        {{ currentStep === steps.length - 2 ? 'Finish' : 'Next' }}
      </button>
    </div>
  </div>
</template>

<script>
import UserForm from './UserForm.vue'

export default {
  name: 'Onboarding',
  components: {
    UserForm
  },
  data() {
    return {
      currentStep: 0,
      userCreatedFlag: false,
      preferences: {
        theme: 'light',
        notifications: true
      },
      steps: [
        { title: 'Welcome' },
        { title: 'Account' },
        { title: 'Preferences' },
        { title: 'Complete' }
      ]
    }
  },
  methods: {
    nextStep() {
      if (this.currentStep < this.steps.length - 1) {
        this.currentStep++
      }
    },
    prevStep() {
      if (this.currentStep > 0) {
        this.currentStep--
      }
    },
    goToStep(index) {
      // Only allow going to steps that are accessible
      if (index < this.currentStep || 
         (index === 1 && this.currentStep === 0) || 
         (index === 2 && this.userCreatedFlag)) {
        this.currentStep = index
      }
    },
    userCreated() {
      this.userCreatedFlag = true
      // Auto advance to next step after creating user
      setTimeout(() => this.nextStep(), 1000)
    },
    completeOnboarding() {
      // Emit event to parent component
      this.$emit('onboarding-completed', {
        preferences: this.preferences
      })
    }
  }
}
</script>

<style scoped>
.onboarding-container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
  background-color: white;
  border-radius: 12px;
  box-shadow: 0 8px 30px rgba(0, 0, 0, 0.05);
}

.step-indicators {
  display: flex;
  justify-content: space-between;
  margin-bottom: 40px;
  position: relative;
}

.step-indicators::before {
  content: "";
  position: absolute;
  top: 20px;
  left: 0;
  right: 0;
  height: 2px;
  background-color: #e0e0e0;
  z-index: 1;
}

.step-indicator {
  position: relative;
  display: flex;
  flex-direction: column;
  align-items: center;
  z-index: 2;
  cursor: pointer;
}

.step-number {
  width: 40px;
  height: 40px;
  background-color: #e0e0e0;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 600;
  margin-bottom: 8px;
  transition: all 0.3s ease;
}

.step-indicator.active .step-number {
  background-color: #42b883;
  color: white;
}

.step-indicator.completed .step-number {
  background-color: #42b883;
  color: white;
}

.step-indicator.completed .step-number::after {
  content: "✓";
}

.step-label {
  font-size: 14px;
  color: #666;
}

.step-indicator.active .step-label {
  color: #42b883;
  font-weight: 600;
}

.step-content {
  min-height: 400px;
  margin-bottom: 30px;
}

.step-navigation {
  display: flex;
  justify-content: space-between;
}

.primary-button {
  padding: 12px 24px;
  background-color: #42b883;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-size: 16px;
  font-weight: 600;
  transition: all 0.2s ease;
}

.primary-button:hover {
  background-color: #3aa876;
  transform: translateY(-2px);
  box-shadow: 0 4px 8px rgba(66, 184, 131, 0.2);
}

.primary-button:disabled {
  background-color: #cccccc;
  cursor: not-allowed;
}

.secondary-button {
  padding: 12px 24px;
  background-color: transparent;
  color: #666;
  border: 1px solid #ddd;
  border-radius: 8px;
  cursor: pointer;
  font-size: 16px;
  font-weight: 600;
  transition: all 0.2s ease;
}

.secondary-button:hover {
  background-color: #f5f5f5;
}

.welcome-image {
  width: 200px;
  height: 200px;
  object-fit: cover;
  border-radius: 50%;
  margin: 20px auto;
  display: block;
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.1);
}

.preferences-form {
  max-width: 500px;
  margin: 0 auto;
}

.theme-options {
  display: flex;
  gap: 10px;
  margin-top: 10px;
}

.theme-option {
  flex: 1;
  padding: 10px;
  border: 2px solid #eee;
  border-radius: 8px;
  background-color: white;
  cursor: pointer;
  transition: all 0.2s ease;
}

.theme-option.selected {
  border-color: #42b883;
  background-color: rgba(66, 184, 131, 0.1);
}

/* Toggle switch styling */
.toggle-switch {
  position: relative;
  width: 60px;
  height: 34px;
  margin-top: 10px;
}

.toggle-switch input {
  opacity: 0;
  width: 0;
  height: 0;
}

.toggle-switch label {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #ccc;
  transition: .4s;
  border-radius: 34px;
}

.toggle-switch label:before {
  position: absolute;
  content: "";
  height: 26px;
  width: 26px;
  left: 4px;
  bottom: 4px;
  background-color: white;
  transition: .4s;
  border-radius: 50%;
}

.toggle-switch input:checked + label {
  background-color: #42b883;
}

.toggle-switch input:checked + label:before {
  transform: translateX(26px);
}

.completion-message {
  text-align: center;
  padding: 20px;
}

/* Checkmark animation */
.checkmark {
  width: 80px;
  height: 80px;
  margin: 0 auto 20px;
}

.checkmark__circle {
  stroke-dasharray: 166;
  stroke-dashoffset: 166;
  stroke-width: 2;
  stroke-miterlimit: 10;
  stroke: #42b883;
  fill: none;
  animation: stroke 0.6s cubic-bezier(0.65, 0, 0.45, 1) forwards;
}

.checkmark__check {
  transform-origin: 50% 50%;
  stroke-dasharray: 48;
  stroke-dashoffset: 48;
  stroke-width: 3;
  stroke: #42b883;
  animation: stroke 0.3s cubic-bezier(0.65, 0, 0.45, 1) 0.8s forwards;
}

@keyframes stroke {
  100% {
    stroke-dashoffset: 0;
  }
}

/* Transition animations */
.fade-enter-active, .fade-leave-active {
  transition: opacity 0.3s, transform 0.3s;
}

.fade-enter-from, .fade-leave-to {
  opacity: 0;
  transform: translateX(10px);
}
</style>