<template>
  <div class="auth-container">
    <div class="auth-form login-form">
      <h2>Login</h2>
      <form @submit.prevent="login">
        <div class="form-group">
          <label for="login-username">Username or Email:</label>
          <input type="text" id="login-username" v-model="loginData.username" required>
        </div>
        <div class="form-group">
          <label for="login-password">Password:</label>
          <input type="password" id="login-password" v-model="loginData.password" required>
        </div>
        <button type="submit">Login</button>
      </form>
    </div>

    <div class="auth-form signup-form">
      <h2>Sign Up</h2>
      <form @submit.prevent="signUp">
        <div class="form-group">
          <label for="signup-username">Username:</label>
          <input type="text" id="signup-username" v-model="signUpData.username" required>
        </div>
        <div class="form-group">
          <label for="signup-email">Email:</label>
          <input type="email" id="signup-email" v-model="signUpData.email" required>
        </div>
        <div class="form-group">
          <label for="signup-password">Password:</label>
          <input type="password" id="signup-password" v-model="signUpData.password" required>
        </div>
        <button type="submit">Sign Up</button>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  data() {
 return {
      loginData: {
        username: '',
        password: ''
      },
      signUpData: {
        username: '',
        email: '',
        password: ''
      }
    };
  },
  methods: {
    login() {
      console.log('Login Data:', this.loginData);
 if (this.loginData.username === 'admin' && this.loginData.password === 'abuse') {
        this.$emit('login-success');
 } else {
 alert('Invalid credentials!');
      }
    },
    signUp() {
      console.log('Sign Up Data:', this.signUpData);
      // Mock sign up call
      alert('Mock sign up attempted!');
    }
  }
};
</script>

<style scoped>
.auth-container {
  display: flex; /* Use flexbox for layout */
  padding: 20px;
  gap: 20px; /* Add some space between the forms */
  flex-wrap: wrap; /* Allow forms to wrap on smaller screens */
  justify-content: center; /* Center forms */
}
.auth-form {
  border: 1px solid #555; /* Darker gray border */
  padding: 20px;
  border-radius: 8px;
  width: 300px;
  background-color: #333; /* Dark gray background for forms */
  color: #eee; /* Light gray text */
}
.auth-form h2 {
  text-align: center;
  margin-bottom: 20px;
  color: #eee; /* Light gray heading color */
}

.form-group {
  margin-bottom: 15px;
}
.form-group input {
  width: 100%;
  padding: 8px;
  box-sizing: border-box;
  background-color: #444; /* Slightly lighter gray for input background */
  border: 1px solid #555; /* Border for input fields */
  color: #eee; /* Light gray text for input */
}

button[type="submit"] {
  width: 100%;
  padding: 10px;
  background-color: #666; /* Gray button background */
  color: #fff; /* White text on button */
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
button:hover {
  background-color: #777; /* Lighter gray on hover */
}
</style>