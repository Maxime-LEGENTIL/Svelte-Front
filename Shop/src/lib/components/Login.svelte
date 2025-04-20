<script>
    import { goto } from '$app/navigation';
    
    // États pour le formulaire
    let email = '';
    let password = '';
    let rememberMe = false;
    let isLoading = false;
    let showPassword = false;
    let formError = '';
    
    // États pour la validation
    let emailTouched = false;
    let passwordTouched = false;
    
    // Fonction de validation
    function validateEmail(email) {
      const regex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
      return regex.test(email);
    }
    
    function validatePassword(password) {
      return password.length >= 6;
    }
    
    // Messages d'erreur
    $: emailError = emailTouched && !validateEmail(email) ? 'Veuillez entrer un email valide' : '';
    $: passwordError = passwordTouched && !validatePassword(password) ? 'Le mot de passe doit contenir au moins 6 caractères' : '';
    
    // Déterminer si le formulaire est valide
    $: isFormValid = validateEmail(email) && validatePassword(password);
    
    // Gérer la soumission du formulaire
    async function handleSubmit() {
      emailTouched = true;
      passwordTouched = true;
      
      if (!isFormValid) return;
      
      try {
        isLoading = true;
        formError = '';
        
        // Simuler un appel API
        await new Promise(resolve => setTimeout(resolve, 1000));
        
        // Rediriger après connexion réussie
        goto('/compte');
      } catch (error) {
        formError = "Une erreur s'est produite lors de la connexion. Veuillez réessayer.";
      } finally {
        isLoading = false;
      }
    }
    
    // Naviguer vers la page d'inscription
    function goToRegister() {
      goto('/inscription');
    }
    
    // Naviguer vers la page de récupération de mot de passe
    function goToForgotPassword() {
      goto('/mot-de-passe-oublie');
    }
    
    // Connexion avec réseaux sociaux
    function socialLogin(provider) {
      // Logique de connexion avec réseaux sociaux
      console.log(`Connexion avec ${provider}`);
    }
  </script>
  
  <div class="auth-page">
    <div class="auth-container">
      <!-- Logo -->
      <div class="logo-container">
        <a href="/" on:click|preventDefault={() => goto('/')}>
          <h1 class="logo">AppMarket</h1>
        </a>
      </div>
      
      <!-- Titre de la page -->
      <h2 class="auth-title">Connexion à votre compte</h2>
      
      <!-- Messages d'erreur du formulaire -->
      {#if formError}
        <div class="form-error">
          <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <circle cx="12" cy="12" r="10"></circle>
            <line x1="12" y1="8" x2="12" y2="12"></line>
            <line x1="12" y1="16" x2="12.01" y2="16"></line>
          </svg>
          {formError}
        </div>
      {/if}
      
      <!-- Formulaire de connexion -->
      <form class="auth-form" on:submit|preventDefault={handleSubmit}>
        <!-- Champ Email -->
        <div class="form-group">
          <label for="email" class="form-label">Email</label>
          <div class="input-container" class:error={emailError}>
            <svg class="input-icon" xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"></path>
              <polyline points="22,6 12,13 2,6"></polyline>
            </svg>
            <input
              type="email"
              id="email"
              class="form-input"
              placeholder="Votre adresse email"
              bind:value={email}
              on:blur={() => emailTouched = true}
            />
          </div>
          {#if emailError}
            <div class="input-error">{emailError}</div>
          {/if}
        </div>
        
        <!-- Champ Mot de passe -->
        <div class="form-group">
          <label for="password" class="form-label">Mot de passe</label>
          <div class="input-container" class:error={passwordError}>
            <svg class="input-icon" xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <rect x="3" y="11" width="18" height="11" rx="2" ry="2"></rect>
              <path d="M7 11V7a5 5 0 0 1 10 0v4"></path>
            </svg>
            <input
              type={showPassword ? 'text' : 'password'}
              id="password"
              class="form-input"
              placeholder="Votre mot de passe"
              bind:value={password}
              on:blur={() => passwordTouched = true}
            />
            <button 
              type="button" 
              class="password-toggle" 
              on:click={() => showPassword = !showPassword}
            >
              {#if showPassword}
                <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                  <path d="M17.94 17.94A10.07 10.07 0 0 1 12 20c-7 0-11-8-11-8a18.45 18.45 0 0 1 5.06-5.94M9.9 4.24A9.12 9.12 0 0 1 12 4c7 0 11 8 11 8a18.5 18.5 0 0 1-2.16 3.19m-6.72-1.07a3 3 0 1 1-4.24-4.24"></path>
                  <line x1="1" y1="1" x2="23" y2="23"></line>
                </svg>
              {:else}
                <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                  <path d="M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z"></path>
                  <circle cx="12" cy="12" r="3"></circle>
                </svg>
              {/if}
            </button>
          </div>
          {#if passwordError}
            <div class="input-error">{passwordError}</div>
          {/if}
        </div>
        
        <!-- Options de connexion -->
        <div class="form-options">
          <label class="checkbox-container">
            <input type="checkbox" bind:checked={rememberMe} />
            <span class="checkmark"></span>
            Se souvenir de moi
          </label>
          <button type="button" class="forgot-link" on:click={goToForgotPassword}>
            Mot de passe oublié?
          </button>
        </div>
        
        <!-- Bouton de connexion -->
        <button 
          type="submit" 
          class="submit-button" 
          disabled={!isFormValid || isLoading}
          class:loading={isLoading}
        >
          {#if isLoading}
            <svg class="spinner" xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <circle cx="12" cy="12" r="10"></circle>
              <path d="M12 6v2"></path>
            </svg>
          {:else}
            Se connecter
          {/if}
        </button>
      </form>
      
      <!-- Séparateur -->
      <div class="separator">
        <span>ou</span>
      </div>
      
      <!-- Connexion avec réseaux sociaux -->
      <div class="social-login">
        <button class="social-button google" on:click={() => socialLogin('Google')}>
          <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24">
            <path d="M12.545 12.151c0 1.054-.855 1.909-1.909 1.909s-1.909-.855-1.909-1.909c0-1.054.855-1.909 1.909-1.909s1.909.855 1.909 1.909zm8.25.661c0 6.213-5.037 11.25-11.25 11.25s-11.25-5.037-11.25-11.25 5.037-11.25 11.25-11.25c3.131 0 5.958 1.282 8.01 3.334l-3.324 3.324c-1.183-1.183-2.808-1.909-4.686-1.909-3.701 0-6.705 3.004-6.705 6.705s3.004 6.705 6.705 6.705c3.701 0 6.705-3.004 6.705-6.705 0-.565-.07-1.115-.201-1.643h-6.504v-4.596h10.99c.172.89.26 1.813.26 2.735z" fill="#4285F4"/>
          </svg>
          <span>Continuer avec Google</span>
        </button>
        <button class="social-button facebook" on:click={() => socialLogin('Facebook')}>
          <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24">
            <path d="M12 0c-6.627 0-12 5.373-12 12s5.373 12 12 12 12-5.373 12-12-5.373-12-12-12zm3 8h-1.35c-.538 0-.65.221-.65.778v1.222h2l-.209 2h-1.791v7h-3v-7h-2v-2h2v-2.308c0-1.769.931-2.692 3.029-2.692h1.971v3z" fill="#1877F2"/>
          </svg>
          <span>Continuer avec Facebook</span>
        </button>
        <button class="social-button apple" on:click={() => socialLogin('Apple')}>
          <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24">
            <path d="M22 17.607c-.786 2.28-3.139 6.317-5.563 6.361-1.608.031-2.125-.953-3.963-.953-1.837 0-2.412.923-3.932.983-2.572.099-6.542-5.827-6.542-10.995 0-4.747 3.308-7.1 6.198-7.143 1.55-.028 3.014 1.045 3.959 1.045.949 0 2.727-1.29 4.596-1.101.782.033 2.979.315 4.389 2.377-3.741 2.442-3.158 7.549.858 9.426zm-5.222-17.607c-2.826.114-5.132 3.079-4.81 5.531 2.612.203 5.118-2.725 4.81-5.531z" fill="#000000"/>
          </svg>
          <span>Continuer avec Apple</span>
        </button>
      </div>
      
      <!-- Lien vers la création de compte -->
      <div class="signup-prompt">
        <p>Vous n'avez pas de compte?</p>
        <button type="button" class="signup-link" on:click={goToRegister}>Créer un compte</button>
      </div>
    </div>
  </div>
  
  <style>
    .auth-page {
      min-height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;
      padding: 2rem;
      background-color: #f8f8f8;
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen-Sans, Ubuntu, Cantarell, 'Helvetica Neue', sans-serif;
    }
    
    .auth-container {
      width: 100%;
      max-width: 420px;
      padding: 2.5rem;
      background-color: white;
      border-radius: 16px;
      box-shadow: 0 4px 20px rgba(0, 0, 0, 0.08);
    }
    
    .logo-container {
      text-align: center;
      margin-bottom: 2rem;
    }
    
    .logo {
      font-size: 1.75rem;
      font-weight: 700;
      margin: 0;
      color: #FF5A5F;
      letter-spacing: -0.5px;
    }
    
    .auth-title {
      font-size: 1.5rem;
      font-weight: 600;
      margin-bottom: 1.5rem;
      text-align: center;
      color: #222222;
    }
    
    .form-error {
      display: flex;
      align-items: center;
      background-color: #FFEBE9;
      color: #D73A49;
      padding: 0.875rem;
      border-radius: 8px;
      margin-bottom: 1.5rem;
      font-size: 0.875rem;
    }
    
    .form-error svg {
      margin-right: 0.5rem;
      flex-shrink: 0;
    }
    
    .auth-form {
      margin-bottom: 1.5rem;
    }
    
    .form-group {
      margin-bottom: 1.25rem;
    }
    
    .form-label {
      display: block;
      margin-bottom: 0.5rem;
      font-size: 0.875rem;
      font-weight: 500;
      color: #484848;
    }
    
    .input-container {
      position: relative;
      display: flex;
      align-items: center;
      border: 1px solid #DDDDDD;
      border-radius: 8px;
      overflow: hidden;
      transition: all 0.2s;
    }
    
    .input-container:focus-within {
      border-color: #FF5A5F;
      box-shadow: 0 0 0 1px #FF5A5F;
    }
    
    .input-container.error {
      border-color: #D73A49;
    }
    
    .input-container.error:focus-within {
      box-shadow: 0 0 0 1px #D73A49;
    }
    
    .input-icon {
      padding: 0 0.75rem;
      color: #717171;
    }
    
    .form-input {
      width: 100%;
      padding: 0.875rem 0;
      border: none;
      background: transparent;
      font-size: 0.95rem;
      color: #222222;
      outline: none;
    }
    
    .form-input::placeholder {
      color: #AAAAAA;
    }
    
    .password-toggle {
      background: none;
      border: none;
      padding: 0.5rem 0.75rem;
      color: #717171;
      cursor: pointer;
      display: flex;
      align-items: center;
    }
    
    .input-error {
      margin-top: 0.5rem;
      color: #D73A49;
      font-size: 0.8rem;
    }
    
    .form-options {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 1.5rem;
      font-size: 0.875rem;
    }
    
    .checkbox-container {
      display: flex;
      align-items: center;
      position: relative;
      padding-left: 28px;
      cursor: pointer;
      user-select: none;
      color: #484848;
    }
    
    .checkbox-container input {
      position: absolute;
      opacity: 0;
      cursor: pointer;
      height: 0;
      width: 0;
    }
    
    .checkmark {
      position: absolute;
      top: 0;
      left: 0;
      height: 18px;
      width: 18px;
      background-color: #FFFFFF;
      border: 1px solid #DDDDDD;
      border-radius: 4px;
      transition: all 0.2s;
    }
    
    .checkbox-container:hover input ~ .checkmark {
      border-color: #FF5A5F;
    }
    
    .checkbox-container input:checked ~ .checkmark {
      background-color: #FF5A5F;
      border-color: #FF5A5F;
    }
    
    .checkmark:after {
      content: "";
      position: absolute;
      display: none;
    }
    
    .checkbox-container input:checked ~ .checkmark:after {
      display: block;
    }
    
    .checkbox-container .checkmark:after {
      left: 6px;
      top: 3px;
      width: 4px;
      height: 8px;
      border: solid white;
      border-width: 0 2px 2px 0;
      transform: rotate(45deg);
    }
    
    .forgot-link {
      background: none;
      border: none;
      color: #FF5A5F;
      cursor: pointer;
      padding: 0;
      font-size: 0.875rem;
      text-decoration: underline;
    }
    
    .forgot-link:hover {
      color: #FF3B41;
    }
    
    .submit-button {
      width: 100%;
      padding: 1rem;
      background-color: #FF5A5F;
      color: white;
      border: none;
      border-radius: 8px;
      font-size: 1rem;
      font-weight: 600;
      cursor: pointer;
      transition: background-color 0.2s;
      display: flex;
      justify-content: center;
      align-items: center;
    }
    
    .submit-button:hover:not(:disabled) {
      background-color: #FF3B41;
    }
    
    .submit-button:disabled {
      background-color: #FFCCCF;
      cursor: not-allowed;
    }
    
    .submit-button.loading {
      background-color: #FF7A7F;
    }
    
    .spinner {
      animation: spin 1s linear infinite;
    }
    
    @keyframes spin {
      0% { transform: rotate(0deg); }
      100% { transform: rotate(360deg); }
    }
    
    .separator {
      display: flex;
      align-items: center;
      text-align: center;
      margin: 1.5rem 0;
      color: #717171;
      font-size: 0.875rem;
    }
    
    .separator::before,
    .separator::after {
      content: '';
      flex: 1;
      border-bottom: 1px solid #EEEEEE;
    }
    
    .separator::before {
      margin-right: 1rem;
    }
    
    .separator::after {
      margin-left: 1rem;
    }
    
    .social-login {
      display: flex;
      flex-direction: column;
      gap: 0.875rem;
      margin-bottom: 1.5rem;
    }
    
    .social-button {
      display: flex;
      align-items: center;
      justify-content: center;
      padding: 0.875rem;
      border: 1px solid #DDDDDD;
      border-radius: 8px;
      background-color: white;
      cursor: pointer;
      font-size: 0.95rem;
      font-weight: 500;
      color: #484848;
      transition: background-color 0.2s, border-color 0.2s;
    }
    
    .social-button:hover {
      background-color: #F9F9F9;
    }
    
    .social-button svg {
      margin-right: 0.75rem;
    }
    
    .signup-prompt {
      text-align: center;
      font-size: 0.9rem;
      color: #484848;
    }
    
    .signup-prompt p {
      margin: 0 0 0.5rem 0;
    }
    
    .signup-link {
      background: none;
      border: none;
      color: #FF5A5F;
      cursor: pointer;
      padding: 0;
      font-weight: 500;
      text-decoration: underline;
    }
    
    .signup-link:hover {
      color: #FF3B41;
    }
    
    /* Responsive design */
    @media (max-width: 480px) {
      .auth-container {
        padding: 1.5rem;
        border-radius: 12px;
      }
      
      .auth-title {
        font-size: 1.25rem;
      }
      
      .form-options {
        flex-direction: column;
        align-items: flex-start;
        gap: 0.875rem;
      }
      
      .forgot-link {
        margin-left: 28px;
      }
    }
  </style>