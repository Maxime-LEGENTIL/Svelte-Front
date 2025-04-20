<script>
  import { onMount } from 'svelte';

  // Variables pour stocker les données du formulaire
  let firstName = '';
  let lastName = '';
  let email = '';
  let phone = '';
  let address = '';
  let password = '';
  let confirmPassword = '';
  let errors = {};
  let loading = false;
  let success = false;

  // Validation du formulaire
  function validateForm() {
    errors = {};
    let isValid = true;

    if (!firstName.trim()) {
      errors.firstName = 'Le prénom est requis';
      isValid = false;
    }

    if (!lastName.trim()) {
      errors.lastName = 'Le nom est requis';
      isValid = false;
    }

    if (!email.trim()) {
      errors.email = 'L\'email est requis';
      isValid = false;
    } else if (!/^\S+@\S+\.\S+$/.test(email)) {
      errors.email = 'Format d\'email invalide';
      isValid = false;
    }

    if (!phone.trim()) {
      errors.phone = 'Le numéro de téléphone est requis';
      isValid = false;
    } else if (!/^[+]?[(]?[0-9]{3}[)]?[-\s.]?[0-9]{3}[-\s.]?[0-9]{4,6}$/.test(phone)) {
      errors.phone = 'Format de téléphone invalide';
      isValid = false;
    }

    if (!address.trim()) {
      errors.address = 'L\'adresse est requise';
      isValid = false;
    }

    if (!password) {
      errors.password = 'Le mot de passe est requis';
      isValid = false;
    } else if (password.length < 8) {
      errors.password = 'Le mot de passe doit contenir au moins 8 caractères';
      isValid = false;
    }

    if (password !== confirmPassword) {
      errors.confirmPassword = 'Les mots de passe ne correspondent pas';
      isValid = false;
    }

    return isValid;
  }

  // Soumission du formulaire
  async function handleSubmit() {
    if (!validateForm()) return;
    
    loading = true;
    
    try {
      const response = await fetch('http://127.0.0.1:8000/api/customers', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
          'Accept': 'application/json'
        },
        body: JSON.stringify({
          "first_name": firstName,
          "last_name": lastName,
          lastName,
          email,
          phone,
          address,
          password,
          password_confirmation: confirmPassword
        })
      });
      
      const data = await response.json();
      
      if (!response.ok) {
        // Gestion des erreurs retournées par l'API
        if (data.errors) {
          errors = data.errors;
        } else {
          errors.general = 'Une erreur est survenue lors de l\'inscription';
        }
      } else {
        // Succès
        success = true;
        firstName = '';
        lastName = '';
        email = '';
        phone = '';
        address = '';
        password = '';
        confirmPassword = '';
      }
    } catch (error) {
      errors.general = 'Erreur de connexion à l\'API';
      console.error('Erreur:', error);
    } finally {
      loading = false;
    }
  }
</script>

<div class="registration-container">
  <div class="registration-card">
    <h1>Créez votre compte</h1>
    <p class="subtitle">Rejoignez notre communauté en quelques clics</p>
    
    {#if success}
      <div class="success-message">
        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
          <path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"></path>
          <polyline points="22 4 12 14.01 9 11.01"></polyline>
        </svg>
        <p>Inscription réussie ! Vous pouvez maintenant vous connecter.</p>
      </div>
    {:else}
      <form on:submit|preventDefault={handleSubmit}>
        {#if errors.general}
          <div class="error-message general-error">{errors.general}</div>
        {/if}
        
        <div class="form-group-row">
          <div class="form-group">
            <label for="firstName">Prénom</label>
            <input 
              type="text" 
              id="firstName" 
              bind:value={firstName} 
              class:input-error={errors.firstName}
              placeholder="Votre prénom" 
            />
            {#if errors.firstName}
              <span class="error-text">{errors.firstName}</span>
            {/if}
          </div>
          
          <div class="form-group">
            <label for="lastName">Nom</label>
            <input 
              type="text" 
              id="lastName" 
              bind:value={lastName} 
              class:input-error={errors.lastName}
              placeholder="Votre nom" 
            />
            {#if errors.lastName}
              <span class="error-text">{errors.lastName}</span>
            {/if}
          </div>
        </div>
        
        <div class="form-group">
          <label for="email">Adresse email</label>
          <input 
            type="email" 
            id="email" 
            bind:value={email} 
            class:input-error={errors.email}
            placeholder="exemple@email.com" 
          />
          {#if errors.email}
            <span class="error-text">{errors.email}</span>
          {/if}
        </div>
        
        <div class="form-group">
          <label for="phone">Téléphone</label>
          <input 
            type="tel" 
            id="phone" 
            bind:value={phone} 
            class:input-error={errors.phone}
            placeholder="Votre numéro de téléphone" 
          />
          {#if errors.phone}
            <span class="error-text">{errors.phone}</span>
          {/if}
        </div>
        
        <div class="form-group">
          <label for="address">Adresse</label>
          <input 
            type="text" 
            id="address" 
            bind:value={address} 
            class:input-error={errors.address}
            placeholder="Votre adresse complète" 
          />
          {#if errors.address}
            <span class="error-text">{errors.address}</span>
          {/if}
        </div>
        
        <div class="form-group">
          <label for="password">Mot de passe</label>
          <input 
            type="password" 
            id="password" 
            bind:value={password} 
            class:input-error={errors.password}
            placeholder="Minimum 8 caractères" 
          />
          {#if errors.password}
            <span class="error-text">{errors.password}</span>
          {/if}
        </div>
        
        <div class="form-group">
          <label for="confirmPassword">Confirmer le mot de passe</label>
          <input 
            type="password" 
            id="confirmPassword" 
            bind:value={confirmPassword} 
            class:input-error={errors.confirmPassword}
            placeholder="Confirmez votre mot de passe" 
          />
          {#if errors.confirmPassword}
            <span class="error-text">{errors.confirmPassword}</span>
          {/if}
        </div>
        
        <button type="submit" class="btn-primary" disabled={loading}>
          {#if loading}
            <span class="loader"></span>
            Inscription en cours...
          {:else}
            S'inscrire
          {/if}
        </button>
        
        <p class="terms-text">
          En vous inscrivant, vous acceptez nos <a href="#">Conditions générales</a> et notre <a href="#">Politique de confidentialité</a>.
        </p>
      </form>
      
      <div class="login-link">
        Vous avez déjà un compte ? <a href="#">Connectez-vous</a>
      </div>
    {/if}
  </div>
</div>

<style>
  .registration-container {
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
    background-color: #f7f7f7;
    padding: 20px;
    font-family: 'Nunito', 'Segoe UI', sans-serif;
  }

  .registration-card {
    background-color: white;
    border-radius: 12px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
    padding: 40px;
    width: 100%;
    max-width: 550px;
  }

  h1 {
    font-size: 28px;
    color: #484848;
    margin-bottom: 8px;
    font-weight: 700;
  }

  .subtitle {
    color: #717171;
    margin-bottom: 32px;
    font-size: 16px;
  }

  .form-group {
    margin-bottom: 24px;
  }

  .form-group-row {
    display: flex;
    gap: 16px;
    margin-bottom: 24px;
  }

  .form-group-row .form-group {
    flex: 1;
    margin-bottom: 0;
  }

  label {
    display: block;
    margin-bottom: 8px;
    font-weight: 600;
    color: #484848;
    font-size: 14px;
  }

  input {
    width: 100%;
    padding: 14px 16px;
    border: 1px solid #DDDDDD;
    border-radius: 8px;
    font-size: 16px;
    transition: all 0.2s ease;
  }

  input::placeholder {
    color: #BBBBBB;
  }

  input:focus {
    outline: none;
    border-color: #FF5A5F;
    box-shadow: 0 0 0 2px rgba(255, 90, 95, 0.2);
  }

  .input-error {
    border-color: #FF5A5F;
  }

  .error-text {
    display: block;
    color: #FF5A5F;
    font-size: 14px;
    margin-top: 6px;
  }

  .btn-primary {
    width: 100%;
    background-color: #FF5A5F;
    color: white;
    border: none;
    border-radius: 8px;
    padding: 16px;
    font-size: 16px;
    font-weight: 600;
    cursor: pointer;
    transition: background-color 0.2s ease;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .btn-primary:hover {
    background-color: #FF3A3F;
  }

  .btn-primary:disabled {
    background-color: #FFACAD;
    cursor: not-allowed;
  }

  .terms-text {
    font-size: 12px;
    color: #717171;
    text-align: center;
    margin-top: 24px;
  }

  .terms-text a {
    color: #FF5A5F;
    text-decoration: none;
  }

  .login-link {
    margin-top: 32px;
    text-align: center;
    font-size: 14px;
    color: #484848;
  }

  .login-link a {
    color: #FF5A5F;
    text-decoration: none;
    font-weight: 600;
  }

  .error-message {
    background-color: #FFF0F0;
    color: #D63031;
    padding: 16px;
    border-radius: 8px;
    margin-bottom: 24px;
    font-size: 14px;
  }

  .success-message {
    display: flex;
    align-items: center;
    background-color: #E3F9E5;
    color: #288A2B;
    padding: 20px;
    border-radius: 8px;
    margin-bottom: 24px;
  }

  .success-message svg {
    margin-right: 12px;
    flex-shrink: 0;
  }

  .loader {
    display: inline-block;
    width: 18px;
    height: 18px;
    border: 2px solid white;
    border-radius: 50%;
    border-top-color: transparent;
    margin-right: 8px;
    animation: spin 0.8s linear infinite;
  }

  @keyframes spin {
    to {
      transform: rotate(360deg);
    }
  }

  @media (max-width: 600px) {
    .registration-card {
      padding: 24px;
    }
    
    .form-group-row {
      flex-direction: column;
      gap: 24px;
    }
  }
</style>