<template>
  <div class="user-form">
    <form @submit.prevent="submitForm">
      <div class="form-group">
        <label for="username">Username</label>
        <div class="input-container">
          <svg class="input-icon" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" width="18" height="18">
            <path d="M12 12a5 5 0 1 0 0-10 5 5 0 0 0 0 10zm0 2c-2.67 0-8 1.34-8 4v2h16v-2c0-2.66-5.33-4-8-4z" fill="currentColor"/>
          </svg>
          <input 
            type="text" 
            id="username" 
            v-model="user.username" 
            placeholder="Enter your username"
            required
          />
        </div>
      </div>
      
      <div class="form-group">
        <label for="email">Email</label>
        <div class="input-container">
          <svg class="input-icon" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" width="18" height="18">
            <path d="M20 4H4a2 2 0 0 0-2 2v12a2 2 0 0 0 2 2h16a2 2 0 0 0 2-2V6a2 2 0 0 0-2-2zm0 4.7l-8 5.334L4 8.7V6.297l8 5.333 8-5.333V8.7z" fill="currentColor"/>
          </svg>
          <input 
            type="email" 
            id="email" 
            v-model="user.email" 
            placeholder="Enter your email"
            required
          />
        </div>
      </div>
      
      <div class="form-group">
        <label for="password">Password</label>
        <div class="input-container">
          <svg class="input-icon" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" width="18" height="18">
            <path d="M18 8h-1V6c0-2.76-2.24-5-5-5S7 3.24 7 6v2H6a2 2 0 0 0-2 2v10a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V10a2 2 0 0 0-2-2zm-6 9a2 2 0 1 1 0-4 2 2 0 0 1 0 4zm3.1-9H8.9V6c0-1.71 1.39-3.1 3.1-3.1 1.71 0 3.1 1.39 3.1 3.1v2z" fill="currentColor"/>
          </svg>
          <input 
            :type="showPassword ? 'text' : 'password'" 
            id="password" 
            v-model="user.password" 
            placeholder="Create a password"
            required
          />
          <button 
            type="button" 
            class="password-toggle" 
            @click="showPassword = !showPassword"
          >
            <svg v-if="!showPassword" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" width="18" height="18">
              <path d="M12 4.5C7 4.5 2.73 7.61 1 12c1.73 4.39 6 7.5 11 7.5s9.27-3.11 11-7.5c-1.73-4.39-6-7.5-11-7.5zM12 17c-2.76 0-5-2.24-5-5s2.24-5 5-5 5 2.24 5 5-2.24 5-5 5zm0-8c-1.66 0-3 1.34-3 3s1.34 3 3 3 3-1.34 3-3-1.34-3-3-3z" fill="currentColor"/>
            </svg>
            <svg v-else xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" width="18" height="18">
              <path d="M12 7c2.76 0 5 2.24 5 5 0 .65-.13 1.26-.36 1.83l2.92 2.92c1.51-1.26 2.7-2.89 3.43-4.75-1.73-4.39-6-7.5-11-7.5-1.4 0-2.74.25-3.98.7l2.16 2.16C10.74 7.13 11.35 7 12 7zM2 4.27l2.28 2.28.46.46C3.08 8.3 1.78 10.02 1 12c1.73 4.39 6 7.5 11 7.5 1.55 0 3.03-.3 4.38-.84l.42.42L19.73 22 21 20.73 3.27 3 2 4.27zM7.53 9.8l1.55 1.55c-.05.21-.08.43-.08.65 0 1.66 1.34 3 3 3 .22 0 .44-.03.65-.08l1.55 1.55c-.67.33-1.41.53-2.2.53-2.76 0-5-2.24-5-5 0-.79.2-1.53.53-2.2zm4.31-.78l3.15 3.15.02-.16c0-1.66-1.34-3-3-3l-.17.01z" fill="currentColor"/>
            </svg>
          </button>
        </div>
      </div>
      
      <div class="form-actions">
        <button type="submit" class="submit-button">
          Create Account
        </button>
      </div>
    </form>
    
    <div v-if="formSubmitted" class="success-message">
      <svg class="success-icon" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" width="24" height="24">
        <path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm-2 15l-5-5 1.41-1.41L10 14.17l7.59-7.59L19 8l-9 9z" fill="#4CAF50"/>
      </svg>
      User created successfully!
    </div>
  </div>
</template>

<script>
export default {
  name: 'UserForm',
  data() {
    return {
      user: {
        username: '',
        email: '',
        password: ''
      },
      formSubmitted: false,
      showPassword: false
    }
  },
  methods: {
    submitForm() {
      // Here you would typically send data to an API
      console.log('Form submitted with data:', this.user);
      this.formSubmitted = true;
      
      // Emit an event to the parent component
      this.$emit('user-created', this.user);
      
      // Reset form after short delay
      setTimeout(() => {
        this.formSubmitted = false;
      }, 3000);
    },
    resetForm() {
      this.user = {
        username: '',
        email: '',
        password: ''
      };
      this.formSubmitted = false;
    }
  }
}
</script>

<style scoped>
.user-form {
  max-width: 500px;
  margin: 0 auto;
  padding: 20px;
}

.form-group {
  margin-bottom: 20px;
}

label {
  display: block;
  margin-bottom: 8px;
  font-weight: 500;
  color: #333;
  font-size: 14px;
}

.input-container {
  position: relative;
  display: flex;
  align-items: center;
}

.input-icon {
  position: absolute;
  left: 12px;
  color: #999;
  pointer-events: none;
}

input {
  width: 100%;
  padding: 12px 12px 12px 40px;
  border: 1px solid #ddd;
  border-radius: 8px;
  font-size: 16px;
  transition: all 0.2s ease;
}

input:focus {
  outline: none;
  border-color: #42b883;
  box-shadow: 0 0 0 2px rgba(66, 184, 131, 0.2);
}

input::placeholder {
  color: #aaa;
}

.password-toggle {
  position: absolute;
  right: 12px;
  background: none;
  border: none;
  cursor: pointer;
  color: #666;
  padding: 0;
}

.form-actions {
  margin-top: 30px;
}

.submit-button {
  width: 100%;
  padding: 12px;
  background-color: #42b883;
  color: white;
  border: none;
  border-radius: 8px;
  font-size: 16px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
}

.submit-button:hover {
  background-color: #3aa876;
  transform: translateY(-2px);
  box-shadow: 0 4px 8px rgba(66, 184, 131, 0.2);
}

.success-message {
  display: flex;
  align-items: center;
  justify-content: center;
  margin-top: 20px;
  padding: 12px;
  background-color: rgba(76, 175, 80, 0.1);
  color: #4CAF50;
  border-radius: 8px;
  font-weight: 500;
}

.success-icon {
  margin-right: 8px;
}
</style>