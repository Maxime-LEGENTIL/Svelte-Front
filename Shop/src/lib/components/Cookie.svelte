<script>
    import { fade } from 'svelte/transition';
    let visible = true;
  
    function acceptCookies() {
      localStorage.setItem("cookiesAccepted", "true");
      visible = false;
    }
  
    function openSettings() {
      alert("Paramètres des cookies à personnaliser");
    }
  
    if (typeof window !== 'undefined' && localStorage.getItem("cookiesAccepted")) {
      visible = false;
    }
  </script>
  
  {#if visible}
    <div class="cookie-banner" in:fade>
      <div class="cookie-text">
        Ce site utilise des cookies pour améliorer votre expérience. En continuant, vous acceptez notre utilisation des cookies.
      </div>
      <div class="cookie-actions">
        <button class="btn-secondary" on:click={openSettings}>Paramétrer</button>
        <button class="btn-primary" on:click={acceptCookies}>Accepter</button>
      </div>
    </div>
  {/if}
  
  <style>
    .cookie-banner {
      position: fixed;
      bottom: 20px;
      left: 50%;
      transform: translateX(-50%);
      background-color: white;
      border: 1px solid #ddd;
      box-shadow: 0 4px 16px rgba(0, 0, 0, 0.1);
      border-radius: 12px;
      padding: 20px;
      max-width: 600px;
      width: 90%;
      z-index: 1000;
      display: flex;
      flex-direction: column;
      gap: 16px;
      font-family: system-ui, sans-serif;
      font-size: 0.95rem;
    }
  
    .cookie-text {
      color: #333;
      line-height: 1.5;
    }
  
    .cookie-actions {
      display: flex;
      justify-content: flex-end;
      gap: 10px;
      flex-wrap: wrap;
    }
  
    .btn-primary {
      background-color: #222;
      color: #fff;
      border: none;
      padding: 10px 18px;
      border-radius: 8px;
      cursor: pointer;
      font-weight: 500;
      transition: background 0.2s ease;
    }
  
    .btn-primary:hover {
      background-color: #444;
    }
  
    .btn-secondary {
      background-color: transparent;
      color: #222;
      border: 1px solid #ccc;
      padding: 10px 18px;
      border-radius: 8px;
      cursor: pointer;
      font-weight: 500;
      transition: background 0.2s ease, border 0.2s ease;
    }
  
    .btn-secondary:hover {
      background-color: #f9f9f9;
      border-color: #bbb;
    }
  
    @media (min-width: 600px) {
      .cookie-banner {
        flex-direction: row;
        align-items: center;
        justify-content: space-between;
      }
  
      .cookie-text {
        flex: 1;
        margin-right: 20px;
      }
    }
  </style>
  