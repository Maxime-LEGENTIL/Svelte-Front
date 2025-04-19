<script>
    import { onMount } from 'svelte';
    import Cookie from './Cookie.svelte';
    
    // État pour contrôler le menu mobile
    let mobileMenuOpen = false;
    
    // Nombre d'articles dans le panier (sera connecté plus tard)
    let itemCount = 2; // Valeur d'exemple
    
    // Fonction pour basculer le menu mobile
    function toggleMobileMenu() {
      mobileMenuOpen = !mobileMenuOpen;
    }
    
    // Fermer le menu mobile si la fenêtre est redimensionnée
    onMount(() => {
      const handleResize = () => {
        if (window.innerWidth > 768 && mobileMenuOpen) {
          mobileMenuOpen = false;
        }
      };
      
      window.addEventListener('resize', handleResize);
      
      return () => {
        window.removeEventListener('resize', handleResize);
      };
    });
    
    // Navigation simplifiée pour l'instant
    function navigate(route) {
      console.log(`Navigation vers ${route}`);
      // Fermer le menu mobile après navigation
      mobileMenuOpen = false;
    }

    let showAccountMenu = false;
let showFavoritesMenu = false;
let showCartMenu = false;

// Gestion du clic en dehors
function closeAllMenus() {
  showAccountMenu = false;
  showFavoritesMenu = false;
  showCartMenu = false;
}

function toggleMenu(menu) {
  closeAllMenus();
  if (menu === 'account') showAccountMenu = !showAccountMenu;
  if (menu === 'favorites') showFavoritesMenu = !showFavoritesMenu;
  if (menu === 'cart') showCartMenu = !showCartMenu;
}

  </script>
  
  <header class="header">
    <div class="header-container">
      <!-- Logo -->
      <div class="logo">
        <a href="/" on:click|preventDefault={() => navigate('/')}>
          <h1>BackMarket</h1>
        </a>
      </div>
      
      <!-- Menu burger pour mobile -->
      <button class="mobile-menu-toggle" on:click={toggleMobileMenu} aria-label="Menu">
        <span class="burger-icon" class:open={mobileMenuOpen}>
          <span></span>
          <span></span>
          <span></span>
        </span>
      </button>
      
      <!-- Navigation et actions utilisateur -->
      <div class="nav-actions-container" class:open={mobileMenuOpen}>
        <!-- Navigation principale -->
        <nav class="main-nav">
          <ul>
            <li><a href="/club" on:click|preventDefault={() => navigate('/club')} class="nav-link">Le Club</a></li>
            <li><a href="/promotions" on:click|preventDefault={() => navigate('/promotions')} class="nav-link">Promotions</a></li>
            <li><a href="/faq" on:click|preventDefault={() => navigate('/faq')} class="nav-link">Besoin d'aide ?</a></li>
          </ul>
        </nav>
        
        <!-- Barre de recherche -->
        <div class="search-container">
          <div class="search-bar">
            <svg class="search-icon" xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <circle cx="11" cy="11" r="8"></circle>
              <line x1="21" y1="21" x2="16.65" y2="16.65"></line>
            </svg>
            <input type="text" placeholder="Rechercher un produit..." />
          </div>
        </div>
        
        <!-- Actions utilisateur -->
        <div class="user-actions">
          <a href="/compte" class="user-action-icon" on:click|preventDefault={() => navigate('/compte')} aria-label="Mon compte">
            <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <path d="M20 21v-2a4 4 0 0 0-4-4H8a4 4 0 0 0-4 4v2"></path>
              <circle cx="12" cy="7" r="4"></circle>
            </svg>
            <span class="action-label">Compte</span>
          </a>
          
          <a href="/favoris" class="user-action-icon" on:click|preventDefault={() => navigate('/favoris')} aria-label="Mes favoris">
            <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <path d="M20.84 4.61a5.5 5.5 0 0 0-7.78 0L12 5.67l-1.06-1.06a5.5 5.5 0 0 0-7.78 7.78l1.06 1.06L12 21.23l7.78-7.78 1.06-1.06a5.5 5.5 0 0 0 0-7.78z"></path>
            </svg>
            <span class="action-label">Favoris</span>
          </a>
          
          <a href="/panier" class="user-action-icon cart-icon" on:click|preventDefault={() => navigate('/panier')} aria-label="Mon panier">
            <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <circle cx="9" cy="21" r="1"></circle>
              <circle cx="20" cy="21" r="1"></circle>
              <path d="M1 1h4l2.68 13.39a2 2 0 0 0 2 1.61h9.72a2 2 0 0 0 2-1.61L23 6H6"></path>
            </svg>
            {#if itemCount > 0}
              <span class="cart-count">{itemCount}</span>
            {/if}
            <span class="action-label">Panier</span>
          </a>
        </div>
      </div>
    </div>
  </header>
  
  <style>
    .header {
      position: sticky;
      top: 0;
      z-index: 1000;
      background-color: white;
      box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
    }
  
    .header-container {
      max-width: 1440px;
      margin: 0 auto;
      padding: 0.75rem 1.5rem;
      display: flex;
      align-items: center;
      position: relative;
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen-Sans, Ubuntu, Cantarell, 'Helvetica Neue', sans-serif;
    }
  
    .logo {
      flex-shrink: 0;
      z-index: 20;
    }
  
    .logo a {
      text-decoration: none;
    }
  
    .logo h1 {
      font-size: 1.5rem;
      font-weight: 700;
      margin: 0;
      color: #FF5A5F; /* Couleur inspirée d'Airbnb */
      letter-spacing: -0.5px;
    }
  
    .nav-actions-container {
      display: flex;
      align-items: center;
      flex-grow: 1;
      justify-content: space-between;
      margin-left: 2rem;
    }
  
    .main-nav ul {
      display: flex;
      list-style: none;
      padding: 0;
      margin: 0;
      gap: 1.5rem;
    }
  
    .nav-link {
      text-decoration: none;
      color: #484848;
      font-weight: 500;
      font-size: 0.95rem;
      padding: 0.5rem 0;
      position: relative;
      transition: color 0.2s;
    }
  
    .nav-link:hover {
      color: #FF5A5F;
    }
  
    .nav-link:after {
      content: '';
      position: absolute;
      bottom: 0;
      left: 0;
      width: 0;
      height: 2px;
      background-color: #FF5A5F;
      transition: width 0.2s ease;
    }
  
    .nav-link:hover:after {
      width: 100%;
    }
  
    .search-container {
      margin: 0 1rem;
      flex-grow: 1;
      max-width: 100ù;
    }
  
    .search-bar {
      display: flex;
      align-items: center;
      border: 1px solid #DDDDDD;
      border-radius: 24px;
      padding: 0.5rem 1rem;
      transition: box-shadow 0.2s, border-color 0.2s;
      background-color: #F7F7F7;
    }
  
    .search-bar:focus-within {
      border-color: #FF5A5F;
      box-shadow: 0 0 0 1px #FF5A5F;
    }
  
    .search-icon {
      color: #717171;
      margin-right: 0.5rem;
    }
  
    .search-bar input {
      border: none;
      background: transparent;
      width: 100%;
      outline: none;
      font-size: 0.9rem;
      color: #222222;
    }
  
    .search-bar input::placeholder {
      color: #717171;
    }
  
    .user-actions {
      display: flex;
      align-items: center;
      gap: 1.25rem;
    }
  
    .user-action-icon {
      display: flex;
      flex-direction: column;
      align-items: center;
      color: #484848;
      text-decoration: none;
      position: relative;
      padding: 0.25rem;
    }
  
    .user-action-icon:hover {
      color: #FF5A5F;
    }
  
    .action-label {
      font-size: 0.7rem;
      margin-top: 0.25rem;
      font-weight: 500;
    }
  
    .cart-count {
      position: absolute;
      top: -5px;
      right: -5px;
      background-color: #FF5A5F;
      color: white;
      font-size: 0.7rem;
      font-weight: 600;
      height: 18px;
      width: 18px;
      display: flex;
      justify-content: center;
      align-items: center;
      border-radius: 50%;
    }
  
    .mobile-menu-toggle {
      display: none;
      background: none;
      border: none;
      cursor: pointer;
      padding: 0.5rem;
      z-index: 20;
    }
  
    .burger-icon {
      width: 24px;
      height: 18px;
      position: relative;
      display: flex;
      flex-direction: column;
      justify-content: space-between;
    }
  
    .burger-icon span {
      display: block;
      height: 2px;
      width: 100%;
      background-color: #484848;
      transition: transform 0.3s, opacity 0.3s;
    }
  
    .burger-icon.open span:nth-child(1) {
      transform: translateY(8px) rotate(45deg);
    }
  
    .burger-icon.open span:nth-child(2) {
      opacity: 0;
    }
  
    .burger-icon.open span:nth-child(3) {
      transform: translateY(-8px) rotate(-45deg);
    }
  
    /* Styles responsives */
    @media (max-width: 992px) {
      .search-container {
        max-width: 300px;
      }
    }
  
    @media (max-width: 768px) {
      .header-container {
        justify-content: space-between;
      }
  
      .mobile-menu-toggle {
        display: block;
      }
  
      .nav-actions-container {
        position: fixed;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        background-color: white;
        margin-left: 0;
        flex-direction: column;
        padding: 5rem 2rem 2rem;
        transform: translateX(-100%);
        transition: transform 0.3s ease;
        z-index: 10;
        overflow-y: auto;
      }
  
      .nav-actions-container.open {
        transform: translateX(0);
      }
  
      .main-nav {
        width: 100%;
      }
  
      .main-nav ul {
        flex-direction: column;
        width: 100%;
        gap: 1rem;
      }
  
      .main-nav li {
        width: 100%;
        border-bottom: 1px solid #EEEEEE;
        padding-bottom: 0.5rem;
      }
  
      .search-container {
        width: 100%;
        max-width: none;
        margin: 1.5rem 0;
      }
  
      .user-actions {
        width: 100%;
        justify-content: space-around;
        padding-top: 1rem;
        border-top: 1px solid #EEEEEE;
      }
    }
  </style>

  <Cookie></Cookie>