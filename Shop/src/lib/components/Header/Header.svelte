<script>
  import { onMount } from 'svelte';
  import { goto } from '$app/navigation'; // Utiliser goto de SvelteKit au lieu de navigate
    import MegaMenu from './MegaMenu.svelte';

  // État pour contrôler le menu mobile
  let mobileMenuOpen = false;

  // Nombre d'articles dans le panier
  let itemCount = 2; // Exemple

  // Paramètres de recherche
  let searchQuery = '';
  let searchFocused = false;
  let recentSearches = ['iPhone 14', 'Samsung TV', 'Casque Bluetooth'];
  let popularCategories = ['Électronique', 'Vêtements', 'Maison', 'Sport', 'Beauté'];
  let suggestedProducts = [
    { id: 1, name: 'iPhone 14 Pro', price: '999€', image: '/images/iphone.jpg' },
    { id: 2, name: 'Samsung Galaxy S23', price: '899€', image: '/images/galaxy.jpg' },
    { id: 3, name: 'AirPods Pro', price: '249€', image: '/images/airpods.jpg' }
  ];

  // Toggle du menu mobile
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
    
    // Ajouter un gestionnaire de clic pour fermer les menus lorsqu'on clique ailleurs
    const handleClickOutside = (event) => {
      if (!event.target.closest('.user-action-icon') && 
          !event.target.closest('.dropdown-menu') &&
          !event.target.closest('.search-container')) {
        closeAllMenus();
        searchFocused = false;
      }
    };
    
    document.addEventListener('click', handleClickOutside);

    return () => {
      window.removeEventListener('resize', handleResize);
      document.removeEventListener('click', handleClickOutside);
    };
  });

  // Navigation via SvelteKit
  function goTo(route) {
    goto(route);
    mobileMenuOpen = false;
    closeAllMenus();
  }

  // Menus (compte, favoris, panier)
  let showAccountMenu = false;
  let showFavoritesMenu = false;
  let showCartMenu = false;

  function closeAllMenus() {
    showAccountMenu = false;
    showFavoritesMenu = false;
    showCartMenu = false;
  }

  function toggleMenu(menu, event) {
    // Empêcher la propagation pour éviter que le menu ne se ferme immédiatement
    if (event) event.stopPropagation();
    
    // Fermer tous les menus puis ouvrir celui demandé
    closeAllMenus();
    if (menu === 'account') showAccountMenu = !showAccountMenu;
    if (menu === 'favorites') showFavoritesMenu = !showFavoritesMenu;
    if (menu === 'cart') showCartMenu = !showCartMenu;
  }
  
  // Focus sur la recherche
  function handleSearchFocus() {
    searchFocused = true;
    closeAllMenus();
  }
  
  // Recherche
  function performSearch() {
    if (searchQuery.trim()) {
      goTo(`/recherche?q=${encodeURIComponent(searchQuery)}`);
      searchFocused = false;
    }
  }
  
  function selectRecentSearch(search) {
    searchQuery = search;
    performSearch();
  }
  
  function goToCategory(category) {
    goTo(`/categorie/${encodeURIComponent(category.toLowerCase())}`);
    searchFocused = false;
  }
  
  function viewProduct(productId) {
    goTo(`/produit/${productId}`);
    searchFocused = false;
  }
</script>

<header class="header">
  <div class="header-container">
    <!-- Logo -->
    <div class="logo">
      <a href="/" on:click|preventDefault={() => goTo('/')}>
        <h1>AppMarket</h1>
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
          <li><a href="/club" on:click|preventDefault={() => goTo('/club')} class="nav-link">Le Club</a></li>
          <li><a href="/promotions" on:click|preventDefault={() => goTo('/promotions')} class="nav-link">Promotions</a></li>
          <li><a href="/faq" on:click|preventDefault={() => goTo('/faq')} class="nav-link">Besoin d'aide ?</a></li>
        </ul>
      </nav>
        
      <!-- Barre de recherche -->
      <div class="search-container">
        <div class="search-bar" class:focused={searchFocused}>
          <svg class="search-icon" xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <circle cx="11" cy="11" r="8"></circle>
            <line x1="21" y1="21" x2="16.65" y2="16.65"></line>
          </svg>
          <input 
            type="text" 
            placeholder="Rechercher un produit..." 
            bind:value={searchQuery}
            on:focus={handleSearchFocus}
            on:keydown={(e) => e.key === 'Enter' && performSearch()}
          />
          {#if searchQuery}
            <button class="search-clear" on:click={() => searchQuery = ''}>
              <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <line x1="18" y1="6" x2="6" y2="18"></line>
                <line x1="6" y1="6" x2="18" y2="18"></line>
              </svg>
            </button>
          {/if}
          <button class="search-submit" on:click={performSearch}>
            Rechercher
          </button>
        </div>
        
        <!-- Interface de recherche avancée -->
        {#if searchFocused}
          <div class="search-dropdown">
            {#if recentSearches.length > 0}
              <div class="search-section">
                <h3>Recherches récentes</h3>
                <ul class="recent-searches">
                  {#each recentSearches as search}
                    <li>
                      <button on:click={() => selectRecentSearch(search)}>
                        <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                          <circle cx="12" cy="12" r="10"></circle>
                          <polyline points="12 6 12 12 16 14"></polyline>
                        </svg>
                        {search}
                      </button>
                    </li>
                  {/each}
                </ul>
              </div>
            {/if}
            
            <div class="search-section">
              <h3>Catégories populaires</h3>
              <div class="category-tags">
                {#each popularCategories as category}
                  <button class="category-tag" on:click={() => goToCategory(category)}>
                    {category}
                  </button>
                {/each}
              </div>
            </div>
            
            <div class="search-section">
              <h3>Produits suggérés</h3>
              <ul class="suggested-products">
                {#each suggestedProducts as product}
                  <li>
                    <button on:click={() => viewProduct(product.id)} class="product-suggestion">
                      <div class="product-image-placeholder"></div>
                      <div class="product-info">
                        <div class="product-name">{product.name}</div>
                        <div class="product-price">{product.price}</div>
                      </div>
                    </button>
                  </li>
                {/each}
              </ul>
            </div>
          </div>
        {/if}
      </div>
        
      <!-- Actions utilisateur -->
      <div class="user-actions">
        <!-- Menu Compte -->
        <div class="user-action-wrapper">
          <button 
            class="user-action-icon" 
            on:click={(e) => toggleMenu('account', e)} 
            aria-label="Mon compte"
            aria-expanded={showAccountMenu}
          >
            <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <path d="M20 21v-2a4 4 0 0 0-4-4H8a4 4 0 0 0-4 4v2"></path>
              <circle cx="12" cy="7" r="4"></circle>
            </svg>
            <span class="action-label">Compte</span>
          </button>
          
          {#if showAccountMenu}
            <div class="dropdown-menu account-menu">
              <div class="dropdown-header">
                <h3>Mon compte</h3>
              </div>
              <div class="dropdown-content">
                {#if false} <!-- Remplacer par une condition pour vérifier si l'utilisateur est connecté -->
                  <div class="welcome-message">
                    <p>Bonjour, <strong>Utilisateur</strong></p>
                  </div>
                  <ul class="menu-links">
                    <li><a href="/compte" on:click|preventDefault={() => goTo('/compte')}>Mon profil</a></li>
                    <li><a href="/commandes" on:click|preventDefault={() => goTo('/commandes')}>Mes commandes</a></li>
                    <li><a href="/adresses" on:click|preventDefault={() => goTo('/adresses')}>Mes adresses</a></li>
                    <li><a href="/moyens-paiement" on:click|preventDefault={() => goTo('/moyens-paiement')}>Moyens de paiement</a></li>
                    <li><a href="/logout" on:click|preventDefault={() => goTo('/logout')}>Se déconnecter</a></li>
                  </ul>
                {:else}
                  <div class="auth-form">
                    <p class="auth-prompt">Connectez-vous pour accéder à votre compte</p>
                    <div class="auth-buttons">
                      <button class="btn-primary" on:click={() => goTo('/connexion')}>Se connecter</button>
                      <button class="btn-outline" on:click={() => goTo('/inscription')}>S'inscrire</button>
                    </div>
                  </div>
                {/if}
              </div>
            </div>
          {/if}
        </div>
        
        <!-- Menu Favoris -->
        <div class="user-action-wrapper">
          <button 
            class="user-action-icon" 
            on:click={(e) => toggleMenu('favorites', e)} 
            aria-label="Mes favoris"
            aria-expanded={showFavoritesMenu}
          >
            <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <path d="M20.84 4.61a5.5 5.5 0 0 0-7.78 0L12 5.67l-1.06-1.06a5.5 5.5 0 0 0-7.78 7.78l1.06 1.06L12 21.23l7.78-7.78 1.06-1.06a5.5 5.5 0 0 0 0-7.78z"></path>
            </svg>
            <span class="action-label">Favoris</span>
          </button>
          
          {#if showFavoritesMenu}
            <div class="dropdown-menu favorites-menu">
              <div class="dropdown-header">
                <h3>Mes favoris</h3>
              </div>
              <div class="dropdown-content">
                <!-- Liste des favoris -->
                <div class="favorites-list">
                  <!-- Si pas de favoris -->
                  <div class="empty-state">
                    <svg xmlns="http://www.w3.org/2000/svg" width="40" height="40" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1" stroke-linecap="round" stroke-linejoin="round">
                      <path d="M20.84 4.61a5.5 5.5 0 0 0-7.78 0L12 5.67l-1.06-1.06a5.5 5.5 0 0 0-7.78 7.78l1.06 1.06L12 21.23l7.78-7.78 1.06-1.06a5.5 5.5 0 0 0 0-7.78z"></path>
                    </svg>
                    <p>Vous n'avez pas encore de favoris</p>
                    <button class="btn-outline" on:click={() => goTo('/produits')}>Découvrir des produits</button>
                  </div>
                </div>
                <!-- Bouton voir tous les favoris -->
                <div class="dropdown-footer">
                  <button class="btn-link" on:click={() => goTo('/favoris')}>Voir tous mes favoris</button>
                </div>
              </div>
            </div>
          {/if}
        </div>
        
        <!-- Menu Panier -->
        <div class="user-action-wrapper">
          <button 
            class="user-action-icon cart-icon" 
            on:click={(e) => toggleMenu('cart', e)}
            aria-label="Mon panier"
            aria-expanded={showCartMenu}
          >
            <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <circle cx="9" cy="21" r="1"></circle>
              <circle cx="20" cy="21" r="1"></circle>
              <path d="M1 1h4l2.68 13.39a2 2 0 0 0 2 1.61h9.72a2 2 0 0 0 2-1.61L23 6H6"></path>
            </svg>
            {#if itemCount > 0}
              <span class="cart-count">{itemCount}</span>
            {/if}
            <span class="action-label">Panier</span>
          </button>
          
          {#if showCartMenu}
            <div class="dropdown-menu cart-menu">
              <div class="dropdown-header">
                <h3>Mon panier ({itemCount} {itemCount > 1 ? 'articles' : 'article'})</h3>
              </div>
              <div class="dropdown-content">
                <!-- Articles dans le panier -->
                <ul class="cart-items">
                  <li class="cart-item">
                    <div class="cart-item-image-placeholder"></div>
                    <div class="cart-item-details">
                      <div class="cart-item-name">Écouteurs Bluetooth Pro</div>
                      <div class="cart-item-price">199,99€</div>
                      <div class="cart-item-quantity">
                        <button class="qty-btn">-</button>
                        <span>1</span>
                        <button class="qty-btn">+</button>
                      </div>
                    </div>
                    <button class="cart-item-remove">
                      <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                        <line x1="18" y1="6" x2="6" y2="18"></line>
                        <line x1="6" y1="6" x2="18" y2="18"></line>
                      </svg>
                    </button>
                  </li>
                  <li class="cart-item">
                    <div class="cart-item-image-placeholder"></div>
                    <div class="cart-item-details">
                      <div class="cart-item-name">Chargeur sans fil 15W</div>
                      <div class="cart-item-price">49,99€</div>
                      <div class="cart-item-quantity">
                        <button class="qty-btn">-</button>
                        <span>1</span>
                        <button class="qty-btn">+</button>
                      </div>
                    </div>
                    <button class="cart-item-remove">
                      <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                        <line x1="18" y1="6" x2="6" y2="18"></line>
                        <line x1="6" y1="6" x2="18" y2="18"></line>
                      </svg>
                    </button>
                  </li>
                </ul>
                
                <!-- Résumé du panier -->
                <div class="cart-summary">
                  <div class="cart-total">
                    <span>Total:</span>
                    <span>249,98€</span>
                  </div>
                </div>
                
                <!-- Boutons d'action -->
                <div class="cart-actions">
                  <button class="btn-primary" on:click={() => goTo('/checkout')}>Passer commande</button>
                  <button class="btn-outline" on:click={() => goTo('/panier')}>Voir mon panier</button>
                </div>
              </div>
            </div>
          {/if}
        </div>
      </div>
    </div>
  </div>
</header>

<MegaMenu></MegaMenu>
  
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
    max-width: 600px;
    position: relative;
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

  .search-bar.focused {
    border-color: #FF5A5F;
    box-shadow: 0 0 0 1px #FF5A5F;
    border-radius: 24px 24px 0 0;
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
  
  .search-clear {
    background: none;
    border: none;
    color: #717171;
    cursor: pointer;
    padding: 0.25rem;
    margin-right: 0.5rem;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  
  .search-clear:hover {
    background-color: #EEEEEE;
  }
  
  .search-submit {
    background-color: #FF5A5F;
    color: white;
    border: none;
    padding: 0.5rem 1rem;
    border-radius: 16px;
    font-weight: 500;
    cursor: pointer;
    font-size: 0.85rem;
    transition: background-color 0.2s;
  }
  
  .search-submit:hover {
    background-color: #FF3B41;
  }
  
  /* Interface de recherche avancée */
  .search-dropdown {
    position: absolute;
    top: 100%;
    left: 0;
    right: 0;
    background-color: white;
    border: 1px solid #FF5A5F;
    border-top: none;
    border-radius: 0 0 24px 24px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
    z-index: 30;
    padding: 1rem;
  }
  
  .search-section {
    margin-bottom: 1.25rem;
  }
  
  .search-section:last-child {
    margin-bottom: 0;
  }
  
  .search-section h3 {
    font-size: 0.85rem;
    color: #717171;
    margin: 0 0 0.75rem 0;
    font-weight: 500;
  }
  
  .recent-searches {
    list-style: none;
    padding: 0;
    margin: 0;
  }
  
  .recent-searches li button {
    background: none;
    border: none;
    display: flex;
    align-items: center;
    padding: 0.5rem;
    width: 100%;
    text-align: left;
    color: #484848;
    border-radius: 8px;
    cursor: pointer;
    transition: background-color 0.2s;
  }
  
  .recent-searches li button:hover {
    background-color: #F7F7F7;
  }
  
  .recent-searches li button svg {
    margin-right: 0.5rem;
    color: #717171;
  }
  
  .category-tags {
    display: flex;
    flex-wrap: wrap;
    gap: 0.5rem;
  }
  
  .category-tag {
    background-color: #F0F0F0;
    border: none;
    padding: 0.5rem 1rem;
    border-radius: 16px;
    font-size: 0.85rem;
    color: #484848;
    cursor: pointer;
    transition: background-color 0.2s, color 0.2s;
  }
  
  .category-tag:hover {
    background-color: #FF5A5F;
    color: white;
  }
  
  .suggested-products {
    list-style: none;
    padding: 0;
    margin: 0;
  }
  
  .product-suggestion {
    display: flex;
    align-items: center;
    padding: 0.5rem;
    width: 100%;
    background: none;
    border: none;
    cursor: pointer;
    text-align: left;
    border-radius: 8px;
    transition: background-color 0.2s;
  }
  
  .product-suggestion:hover {
    background-color: #F7F7F7;
  }
  
  .product-image-placeholder {
    width: 40px;
    height: 40px;
    border-radius: 4px;
    background-color: #E0E0E0;
    margin-right: 0.75rem;
    flex-shrink: 0;
  }
  
  .product-info {
    flex-grow: 1;
  }
  
  .product-name {
    font-size: 0.9rem;
    color: #222222;
    margin-bottom: 0.25rem;
  }
  
  .product-price {
    font-size: 0.85rem;
    font-weight: 600;
    color: #FF5A5F;
  }

  .user-actions {
    display: flex;
    align-items: center;
    gap: 1.25rem;
  }
  
  .user-action-wrapper {
    position: relative;
  }

  .user-action-icon {
    display: flex;
    flex-direction: column;
    align-items: center;
    color: #484848;
    text-decoration: none;
    background: none;
    border: none;
    cursor: pointer;
    padding: 0.25rem;
    transition: color 0.2s;
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
  
  /* Menus déroulants */
  .dropdown-menu {
    position: absolute;
    top: calc(100% + 0.5rem);
    right: -80px;
    width: 320px;
    background-color: white;
    border-radius: 8px;
    box-shadow: 0 4px 20px rgba(0, 0, 0, 0.15);
    z-index: 50;
    overflow: hidden;
    animation: dropdown-appear 0.2s ease;
  }
  
  @keyframes dropdown-appear {
    from {
      opacity: 0;
      transform: translateY(-10px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }
  
  .dropdown-header {
    padding: 1rem 1rem 0.5rem;
    border-bottom: 1px solid #EEEEEE;
  }
  
  .dropdown-header h3 {
    margin: 0;
    font-size: 1rem;
    color: #222222;
    font-weight: 600;
  }
  
  .dropdown-content {
    padding: 1rem;
  }
  
  .dropdown-footer {
    padding: 0.75rem 1rem;
    border-top: 1px solid #EEEEEE;
    text-align: center;
  }
  
  /* Menu Compte */
  .welcome-message {
    margin-bottom: 1rem;
    font-size: 0.9rem;
  }
  
  .welcome-message p {
    margin: 0;
  }
  
  .menu-links {
    list-style: none;
    padding: 0;
    margin: 0;
  }
  
  .menu-links li {
    border-bottom: 1px solid #EEEEEE;
  }
  
  .menu-links li:last-child {
    border-bottom: none;
  }
  
  .menu-links a {
    display: block;
    padding: 0.75rem 0;
    color: #484848;
    text-decoration: none;
    transition: color 0.2s;
  }
  
  .menu-links a:hover {
    color: #FF5A5F;
  }
  
  .auth-form {
    text-align: center;
  }
  
  .auth-prompt {
    margin-bottom: 1rem;
    font-size: 0.9rem;
    color: #484848;
  }
  
  .auth-buttons {
    display: flex;
    gap: 0.75rem;
  }
  
  /* Menu Favoris */
  .empty-state {
    text-align: center;
    padding: 1.5rem 0;
  }
  
  .empty-state svg {
    margin-bottom: 0.75rem;
    color: #CCCCCC;
  }
  
  .empty-state p {
    color: #717171;
    margin-bottom: 1rem;
  }
  
  /* Menu Panier */
  .cart-items {
    list-style: none;
    padding: 0;
    margin: 0 0 1rem 0;
    max-height: 250px;
    overflow-y: auto;
  }
  
  .cart-item {
    display: flex;
    align-items: center;
    padding: 0.75rem 0;
    border-bottom: 1px solid #EEEEEE;
  }
  
  .cart-item:last-child {
    border-bottom: none;
  }
  
  .cart-item-image-placeholder {
    width: 50px;
    height: 50px;
    background-color: #E0E0E0;
    border-radius: 4px;
    margin-right: 0.75rem;
    flex-shrink: 0;
  }
  
  .cart-item-details {
    flex-grow: 1;
  }
  
  .cart-item-name {
    font-size: 0.9rem;
    color: #222222;
    margin-bottom: 0.25rem;
  }
  
  .cart-item-price {
    font-size: 0.85rem;
    font-weight: 600;
    color: #FF5A5F;
    margin-bottom: 0.5rem;
  }
  
  .cart-item-quantity {
    display: flex;
    align-items: center;
  }
  
  .qty-btn {
    width: 24px;
    height: 24px;
    background-color: #F0F0F0;
    border: none;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    font-weight: bold;
    cursor: pointer;
  }
  
  .cart-item-quantity span {
    margin: 0 0.5rem;
    font-size: 0.85rem;
  }
  
  .cart-item-remove {
    background: none;
    border: none;
    color: #717171;
    cursor: pointer;
    padding: 0.25rem;
    margin-left: 0.5rem;
  }
  
  .cart-summary {
    padding: 0.75rem 0;
    border-top: 1px solid #EEEEEE;
    margin-bottom: 1rem;
  }
  
  .cart-total {
    display: flex;
    justify-content: space-between;
    font-weight: 600;
    font-size: 1rem;
  }
  
  .cart-actions {
    display: flex;
    flex-direction: column;
    gap: 0.75rem;
  }
  
  /* Boutons génériques */
  .btn-primary {
    background-color: #FF5A5F;
    color: white;
    border: none;
    padding: 0.75rem 1rem;
    border-radius: 8px;
    font-weight: 500;
    cursor: pointer;
    width: 100%;
    transition: background-color 0.2s;
  }
  
  .btn-primary:hover {
    background-color: #FF3B41;
  }
  
  .btn-outline {
    background-color: white;
    color: #484848;
    border: 1px solid #DDDDDD;
    padding: 0.75rem 1rem;
    border-radius: 8px;
    font-weight: 500;
    cursor: pointer;
    width: 100%;
    transition: border-color 0.2s, color 0.2s;
  }
  
  .btn-outline:hover {
    border-color: #FF5A5F;
    color: #FF5A5F;
  }
  
  .btn-link {
    background: none;
    border: none;
    color: #FF5A5F;
    text-decoration: underline;
    padding: 0.5rem;
    cursor: pointer;
    font-size: 0.9rem;
    transition: color 0.2s;
  }
  
  .btn-link:hover {
    color: #FF3B41;
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
    
    .dropdown-menu {
      width: 280px;
      right: -50px;
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
    
    .search-dropdown {
      position: fixed;
      left: 2rem;
      right: 2rem;
      z-index: 60;
    }

    .user-actions {
      width: 100%;
      justify-content: space-around;
      padding-top: 1rem;
      border-top: 1px solid #EEEEEE;
    }
    
    .dropdown-menu {
      position: fixed;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      width: 90%;
      max-width: 350px;
      max-height: 80vh;
      overflow-y: auto;
      box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
    }
  }
</style>