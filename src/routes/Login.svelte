<script>
    import { onMount } from 'svelte';
  
    let username = '';
    let password = '';
    let errorMessage = '';
  
    function handleLogin() {
      // Implement your login logic here, e.g., calling an API to authenticate the user
      if (username === 'admin' && password === 'password') {
        errorMessage = '';  // Clear any previous error messages
        // Redirect to another page or take appropriate action on successful login
        window.location.href = '/dashboard'; // Example redirect
      } else {
        errorMessage = 'Invalid username or password';
      }
    }
  </script>
  
  <style>
    .login-container {
      display: flex;
      flex-direction: column;
      align-items: center;
      padding: 20px;
      max-width: 400px;
      margin: 50px auto;
      background-color: var(--background-color);
      border-radius: 8px;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
    }
    .login-container h2 {
      margin-bottom: 20px;
      color: var(--text-color);
    }
    .input-field {
      width: 100%;
      margin-bottom: 15px;
    }
    .input-field label {
      display: block;
      margin-bottom: 5px;
      color: var(--text-color);
    }
    .input-field input {
      width: 100%;
      padding: 10px;
      border: 1px solid var(--input-border-color);
      border-radius: 5px;
      background-color: var(--input-background-color);
      color: var(--text-color);
    }
    .login-button {
      width: 100%;
      padding: 10px;
      background-color: var(--button-background-color);
      color: var(--button-text-color);
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }
    .login-button:hover {
      background-color: var(--button-hover-background-color);
    }
    .error-message {
      color: var(--error-color);
      margin-top: 10px;
    }
  </style>
  
  <div class="login-container">
    <h2>Login</h2>
  
    <div class="input-field">
      <label for="username">Username</label>
      <input type="text" id="username" bind:value={username} />
    </div>
  
    <div class="input-field">
      <label for="password">Password</label>
      <input type="password" id="password" bind:value={password} />
    </div>
  
    <button class="login-button" on:click={handleLogin}>Login</button>
  
    {#if errorMessage}
      <div class="error-message">{errorMessage}</div>
    {/if}
  </div>
  