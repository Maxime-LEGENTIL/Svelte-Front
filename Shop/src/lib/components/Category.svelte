<script>
    import { onMount } from 'svelte';
  
    // Props du composant
    export let categoryName = "Chaussures de randonnée";
    export let categoryDescription = "Découvrez notre sélection de chaussures de randonnée pour tous les terrains et toutes les saisons.";
    
    // Données de produits (normalement chargées depuis une API)
    let products = [
      {
        id: 1,
        name: "Chaussures de randonnée premium",
        price: 129.99,
        originalPrice: 159.99,
        rating: 4.7,
        reviewCount: 142,
        image: "/api/placeholder/300/300",
        brand: "Outdoor Pro",
        features: ["Imperméable", "Semelle anti-dérapante"],
        inStock: true,
        badges: ["Populaire", "Promo"],
        colors: ["Noir", "Gris", "Bleu"]
      },
      {
        id: 2,
        name: "Chaussures de trail légères",
        price: 89.99,
        originalPrice: null,
        rating: 4.3,
        reviewCount: 78,
        image: "/api/placeholder/300/300",
        brand: "Mountain Runner",
        features: ["Léger", "Respirant"],
        inStock: true,
        badges: ["Nouveauté"],
        colors: ["Rouge", "Noir"]
      },
      {
        id: 3,
        name: "Bottes de randonnée étanches",
        price: 149.99,
        originalPrice: 179.99,
        rating: 4.8,
        reviewCount: 214,
        image: "/api/placeholder/300/300",
        brand: "AlpineTrek",
        features: ["100% Étanche", "Isolation thermique"],
        inStock: false,
        badges: ["Meilleure vente"],
        colors: ["Marron", "Kaki"]
      },
      {
        id: 4,
        name: "Chaussures d'approche",
        price: 119.99,
        originalPrice: null,
        rating: 4.5,
        reviewCount: 56,
        image: "/api/placeholder/300/300",
        brand: "ClimbTech",
        features: ["Adhérence sur rochers", "Confort urbain"],
        inStock: true,
        badges: [],
        colors: ["Gris", "Vert"]
      },
      {
        id: 5,
        name: "Sandales de randonnée",
        price: 59.99,
        originalPrice: 79.99,
        rating: 4.2,
        reviewCount: 103,
        image: "/api/placeholder/300/300",
        brand: "Outdoor Pro",
        features: ["Séchage rapide", "Semelle antidérapante"],
        inStock: true,
        badges: ["Promo"],
        colors: ["Noir", "Bleu", "Vert"]
      },
      {
        id: 6,
        name: "Chaussures d'hiver isolées",
        price: 169.99,
        originalPrice: null,
        rating: 4.9,
        reviewCount: 87,
        image: "/api/placeholder/300/300",
        brand: "AlpineTrek",
        features: ["Isolé", "Étanche", "Anti-froid"],
        inStock: true,
        badges: ["Premium"],
        colors: ["Noir"]
      }
    ];
  
    // État des filtres et du tri
    let filteredProducts = [...products];
    let searchQuery = "";
    let sortOption = "popular";
    let filterBrand = [];
    let filterPrice = {
      min: 0,
      max: 200
    };
    let filterAvailability = false;
    let filterOnSale = false;
    let selectedColors = [];
    let viewMode = "grid"; // grid ou list
    let itemsPerPage = 12;
    let currentPage = 1;
  
    // Liste de marques pour le filtre
    $: brands = [...new Set(products.map(product => product.brand))];
    
    // Liste de couleurs pour le filtre
    $: availableColors = [...new Set(products.flatMap(product => product.colors))];
  
    // Fonction pour appliquer les filtres et le tri
    function applyFiltersAndSort() {
      // Filtrage
      filteredProducts = products.filter(product => {
        // Recherche par nom
        const matchesSearch = product.name.toLowerCase().includes(searchQuery.toLowerCase());
        
        // Filtre par marque
        const matchesBrand = filterBrand.length === 0 || filterBrand.includes(product.brand);
        
        // Filtre par prix
        const matchesPrice = product.price >= filterPrice.min && product.price <= filterPrice.max;
        
        // Filtre par disponibilité
        const matchesAvailability = !filterAvailability || product.inStock;
        
        // Filtre par promotion
        const matchesOnSale = !filterOnSale || (product.originalPrice && product.originalPrice > product.price);
        
        // Filtre par couleur
        const matchesColor = selectedColors.length === 0 || 
          selectedColors.some(color => product.colors.includes(color));
        
        return matchesSearch && matchesBrand && matchesPrice && matchesAvailability && matchesOnSale && matchesColor;
      });
      
      // Tri
      filteredProducts.sort((a, b) => {
        switch (sortOption) {
          case "price-low":
            return a.price - b.price;
          case "price-high":
            return b.price - a.price;
          case "rating":
            return b.rating - a.rating;
          case "newest":
            return b.id - a.id;
          case "popular":
          default:
            return b.reviewCount - a.reviewCount;
        }
      });
      
      // Réinitialiser la page
      currentPage = 1;
    }
    
    // Calcul des produits à afficher en fonction de la pagination
    $: paginatedProducts = filteredProducts.slice(
      (currentPage - 1) * itemsPerPage, 
      currentPage * itemsPerPage
    );
    
    // Calcul du nombre total de pages
    $: totalPages = Math.ceil(filteredProducts.length / itemsPerPage);
    
    // Fonction pour changer de page
    function goToPage(page) {
      currentPage = page;
      window.scrollTo({ top: 0, behavior: 'smooth' });
    }
    
    // Formater les prix
    function formatPrice(price) {
      return new Intl.NumberFormat('fr-FR', { style: 'currency', currency: 'EUR' }).format(price);
    }
    
    // Effectuer un filtrage initial au chargement
    onMount(() => {
      applyFiltersAndSort();
    });
    
    // Observer les changements dans les filtres et le tri
    $: {
      searchQuery, sortOption, filterBrand, filterPrice, filterAvailability, filterOnSale, selectedColors;
      applyFiltersAndSort();
    }
    
    // Fonction pour ajouter au panier
    function addToCart(product) {
      alert(`${product.name} ajouté au panier`);
    }
    
    // Fonction pour ajouter aux favoris
    function addToWishlist(product) {
      alert(`${product.name} ajouté aux favoris`);
    }
    
    // Génère un tableau de nombres pour les pages à afficher
    function generatePageNumbers(current, total) {
      if (total <= 5) {
        return Array.from({ length: total }, (_, i) => i + 1);
      }
      
      if (current <= 3) {
        return [1, 2, 3, 4, '...', total];
      }
      
      if (current >= total - 2) {
        return [1, '...', total - 3, total - 2, total - 1, total];
      }
      
      return [1, '...', current - 1, current, current + 1, '...', total];
    }
    
    $: pageNumbers = generatePageNumbers(currentPage, totalPages);
  </script>
  
  <div class="category-container">
    <!-- En-tête de la catégorie -->
    <div class="category-header">
      <h1>{categoryName}</h1>
      <p>{categoryDescription}</p>
    </div>
  
    <div class="category-layout">
      <!-- Sidebar avec filtres -->
      <aside class="filters-sidebar">
        <div class="filter-section">
          <h3>Recherche</h3>
          <input 
            type="text" 
            placeholder="Rechercher..." 
            bind:value={searchQuery}
            class="search-input"
          />
        </div>
        
        <div class="filter-section">
          <h3>Prix</h3>
          <div class="price-range">
            <input 
              type="range" 
              bind:value={filterPrice.min} 
              min="0" 
              max={filterPrice.max} 
              step="5"
              class="price-slider"
            />
            <div class="price-inputs">
              <input type="number" bind:value={filterPrice.min} min="0" max={filterPrice.max} />
              <span>à</span>
              <input type="number" bind:value={filterPrice.max} min={filterPrice.min} max="1000" />
            </div>
            <div class="price-labels">
              <span>{formatPrice(filterPrice.min)}</span>
              <span>{formatPrice(filterPrice.max)}</span>
            </div>
          </div>
        </div>
        
        <div class="filter-section">
          <h3>Marque</h3>
          <div class="brand-options">
            {#each brands as brand}
              <label class="filter-option">
                <input 
                  type="checkbox" 
                  bind:group={filterBrand} 
                  value={brand}
                />
                <span>{brand}</span>
              </label>
            {/each}
          </div>
        </div>
        
        <div class="filter-section">
          <h3>Couleurs</h3>
          <div class="color-options">
            {#each availableColors as color}
              <label 
                class="color-option {selectedColors.includes(color) ? 'selected' : ''}"
                style="--color: {color.toLowerCase()}"
              >
                <input 
                  type="checkbox" 
                  bind:group={selectedColors} 
                  value={color}
                />
                <span class="color-swatch"></span>
                <span class="color-name">{color}</span>
              </label>
            {/each}
          </div>
        </div>
        
        <div class="filter-section options">
          <label class="filter-option">
            <input type="checkbox" bind:checked={filterAvailability} />
            <span>En stock uniquement</span>
          </label>
          
          <label class="filter-option">
            <input type="checkbox" bind:checked={filterOnSale} />
            <span>En promotion</span>
          </label>
        </div>
        
        <button class="reset-filters" on:click={() => {
          searchQuery = "";
          filterBrand = [];
          filterPrice = { min: 0, max: 200 };
          filterAvailability = false;
          filterOnSale = false;
          selectedColors = [];
          sortOption = "popular";
        }}>
          Réinitialiser les filtres
        </button>
      </aside>
      
      <!-- Contenu principal -->
      <div class="products-container">
        <!-- Barre d'outils au-dessus des produits -->
        <div class="products-toolbar">
          <div class="results-count">
            <span>{filteredProducts.length} produits trouvés</span>
          </div>
          
          <div class="sort-options">
            <label for="sort-select">Trier par:</label>
            <select id="sort-select" bind:value={sortOption}>
              <option value="popular">Popularité</option>
              <option value="price-low">Prix: croissant</option>
              <option value="price-high">Prix: décroissant</option>
              <option value="rating">Avis clients</option>
              <option value="newest">Nouveautés</option>
            </select>
          </div>
          
          <div class="view-options">
            <button 
              class={viewMode === 'grid' ? 'active' : ''} 
              on:click={() => viewMode = 'grid'}
            >
              <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <rect x="3" y="3" width="7" height="7"></rect>
                <rect x="14" y="3" width="7" height="7"></rect>
                <rect x="3" y="14" width="7" height="7"></rect>
                <rect x="14" y="14" width="7" height="7"></rect>
              </svg>
            </button>
            <button 
              class={viewMode === 'list' ? 'active' : ''} 
              on:click={() => viewMode = 'list'}
            >
              <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <line x1="8" y1="6" x2="21" y2="6"></line>
                <line x1="8" y1="12" x2="21" y2="12"></line>
                <line x1="8" y1="18" x2="21" y2="18"></line>
                <line x1="3" y1="6" x2="3.01" y2="6"></line>
                <line x1="3" y1="12" x2="3.01" y2="12"></line>
                <line x1="3" y1="18" x2="3.01" y2="18"></line>
              </svg>
            </button>
          </div>
        </div>
        
        <!-- Affichage des produits -->
        {#if filteredProducts.length === 0}
          <div class="no-products">
            <svg xmlns="http://www.w3.org/2000/svg" width="48" height="48" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
              <circle cx="11" cy="11" r="8"></circle>
              <line x1="21" y1="21" x2="16.65" y2="16.65"></line>
              <line x1="8" y1="11" x2="14" y2="11"></line>
            </svg>
            <h3>Aucun produit trouvé</h3>
            <p>Essayez de modifier vos critères de recherche</p>
          </div>
        {:else}
          <div class="products-grid {viewMode === 'list' ? 'list-view' : ''}">
            {#each paginatedProducts as product (product.id)}
              <div class="product-card">
                <!-- Badges -->
                {#if product.badges && product.badges.length > 0}
                  <div class="product-badges">
                    {#each product.badges as badge}
                      <span class="badge">{badge}</span>
                    {/each}
                  </div>
                {/if}
                
                <!-- Image -->
                <div class="product-image">
                  <img src="https://www.backmarket.fr/cdn-cgi/image/format%3Dauto%2Cquality%3D75%2Cwidth%3D640/https://d2e6ccujb3mkqf.cloudfront.net/de600cf5-603a-430e-8dac-bc55f2ed2960-1_461a04dc-6e6f-444b-9a05-ce621ba5f5ae.jpg" alt={product.name} />
                  <button class="wishlist-button" on:click={() => addToWishlist(product)}>
                    <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                      <path d="M20.84 4.61a5.5 5.5 0 0 0-7.78 0L12 5.67l-1.06-1.06a5.5 5.5 0 0 0-7.78 7.78l1.06 1.06L12 21.23l7.78-7.78 1.06-1.06a5.5 5.5 0 0 0 0-7.78z"></path>
                    </svg>
                  </button>
                </div>
                
                <!-- Informations -->
                <div class="product-info">
                  <div class="product-brand">{product.brand}</div>
                  <h3 class="product-name">{product.name}</h3>
                  
                  <!-- Prix -->
                  <div class="product-pricing">
                    <span class="product-price">{formatPrice(product.price)}</span>
                    {#if product.originalPrice}
                      <span class="product-original-price">{formatPrice(product.originalPrice)}</span>
                      <span class="product-discount">
                        -{Math.round((1 - product.price / product.originalPrice) * 100)}%
                      </span>
                    {/if}
                  </div>
                  
                  <!-- Notes et avis -->
                  <div class="product-rating">
                    <div class="rating-stars">
                      {#each Array(5) as _, i}
                        <svg 
                          xmlns="http://www.w3.org/2000/svg" 
                          width="12" 
                          height="12" 
                          viewBox="0 0 24 24" 
                          fill={i < Math.floor(product.rating) ? "currentColor" : "none"} 
                          stroke="currentColor" 
                          stroke-width="2"
                        >
                          <polygon points="12 2 15.09 8.26 22 9.27 17 14.14 18.18 21.02 12 17.77 5.82 21.02 7 14.14 2 9.27 8.91 8.26 12 2" />
                        </svg>
                      {/each}
                      <span>{product.rating}</span>
                    </div>
                    <span class="review-count">({product.reviewCount})</span>
                  </div>
                  
                  <!-- Caractéristiques (en mode liste) -->
                  {#if viewMode === 'list' && product.features.length > 0}
                    <div class="product-features">
                      <ul>
                        {#each product.features as feature}
                          <li>{feature}</li>
                        {/each}
                      </ul>
                    </div>
                  {/if}
                  
                  <!-- Disponibilité -->
                  <div class="product-availability {product.inStock ? 'in-stock' : 'out-of-stock'}">
                    {#if product.inStock}
                      <span>En stock</span>
                    {:else}
                      <span>Rupture de stock</span>
                    {/if}
                  </div>
                  
                  <!-- Couleurs disponibles -->
                  <div class="product-colors">
                    {#each product.colors as color}
                      <span class="color-dot" style="--color: {color.toLowerCase()}" title={color}></span>
                    {/each}
                    <span class="color-count">{product.colors.length} couleur{product.colors.length > 1 ? 's' : ''}</span>
                  </div>
                  
                  <!-- Actions -->
                  <div class="product-actions">
                    <button 
                      class="add-to-cart-btn" 
                      on:click={() => addToCart(product)}
                      disabled={!product.inStock}
                    >
                      {product.inStock ? 'Ajouter au panier' : 'Indisponible'}
                    </button>
                    {#if viewMode === 'list'}
                      <a href={`/produit/${product.id}`} class="view-details-btn">
                        Voir les détails
                      </a>
                    {/if}
                  </div>
                </div>
                
                {#if viewMode === 'grid'}
                  <a href={`/produit/${product.id}`} class="quick-view-overlay">
                    <span>Voir le produit</span>
                  </a>
                {/if}
              </div>
            {/each}
          </div>
        {/if}
        
        <!-- Pagination -->
        {#if totalPages > 1}
          <div class="pagination">
            <button 
              class="pagination-arrow"
              disabled={currentPage === 1}
              on:click={() => goToPage(currentPage - 1)}
            >
              <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <polyline points="15 18 9 12 15 6"></polyline>
              </svg>
            </button>
            
            {#each pageNumbers as page}
              {#if page === '...'}
                <span class="pagination-ellipsis">...</span>
              {:else}
                <button 
                  class="pagination-page {currentPage === page ? 'active' : ''}"
                  on:click={() => goToPage(page)}
                >
                  {page}
                </button>
              {/if}
            {/each}
            
            <button 
              class="pagination-arrow"
              disabled={currentPage === totalPages}
              on:click={() => goToPage(currentPage + 1)}
            >
              <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <polyline points="9 18 15 12 9 6"></polyline>
              </svg>
            </button>
          </div>
        {/if}
      </div>
    </div>
  </div>
  
  <style>
    .category-container {
      width: 100%;
      max-width: 1400px;
      margin: 0 auto;
      padding: 20px;
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
    }
  
    .category-header {
      margin-bottom: 30px;
      text-align: center;
    }
  
    .category-header h1 {
      font-size: 32px;
      margin-bottom: 12px;
    }
  
    .category-header p {
      font-size: 16px;
      color: #666;
      max-width: 800px;
      margin: 0 auto;
    }
  
    .category-layout {
      display: flex;
      gap: 30px;
    }
  
    /* Sidebar de filtres */
    .filters-sidebar {
      width: 250px;
      flex-shrink: 0;
    }
  
    .filter-section {
      margin-bottom: 24px;
      padding-bottom: 20px;
      border-bottom: 1px solid #eee;
    }
  
    .filter-section h3 {
      font-size: 16px;
      margin-bottom: 12px;
      font-weight: 600;
    }
  
    .search-input {
      width: 100%;
      padding: 10px;
      border: 1px solid #ddd;
      border-radius: 6px;
    }
  
    .price-range {
      padding: 0 10px;
    }
  
    .price-slider {
      width: 100%;
      margin: 10px 0;
    }
  
    .price-inputs {
      display: flex;
      align-items: center;
      justify-content: space-between;
      margin-bottom: 8px;
    }
  
    .price-inputs input {
      width: 70px;
      padding: 8px;
      border: 1px solid #ddd;
      border-radius: 4px;
    }
  
    .price-inputs span {
      color: #666;
    }
  
    .price-labels {
      display: flex;
      justify-content: space-between;
      font-size: 12px;
      color: #666;
    }
  
    .brand-options {
      display: flex;
      flex-direction: column;
      gap: 8px;
    }
  
    .filter-option {
      display: flex;
      align-items: center;
      gap: 8px;
      cursor: pointer;
    }
  
    .filter-option span {
      font-size: 14px;
    }
  
    .color-options {
      display: flex;
      flex-wrap: wrap;
      gap: 8px;
    }
  
    .color-option {
      display: flex;
      align-items: center;
      gap: 6px;
      cursor: pointer;
      padding: 4px 8px;
      border-radius: 4px;
      transition: background-color 0.2s;
    }
  
    .color-option:hover {
      background-color: #f5f5f5;
    }
  
    .color-option.selected {
      background-color: #f0f0f0;
    }
  
    .color-option input {
      display: none;
    }
  
    .color-swatch {
      width: 16px;
      height: 16px;
      border-radius: 50%;
      background-color: var(--color, #888);
      border: 1px solid #ddd;
    }
  
    .color-name {
      font-size: 14px;
    }
  
    .reset-filters {
      width: 100%;
      padding: 10px;
      background-color: #f5f5f5;
      border: 1px solid #ddd;
      border-radius: 6px;
      cursor: pointer;
      font-size: 14px;
      transition: background-color 0.2s;
    }
  
    .reset-filters:hover {
      background-color: #e5e5e5;
    }
  
    /* Conteneur principal de produits */
    .products-container {
      flex-grow: 1;
    }
  
    .products-toolbar {
      display: flex;
      align-items: center;
      justify-content: space-between;
      margin-bottom: 20px;
      padding-bottom: 15px;
      border-bottom: 1px solid #eee;
    }
  
    .results-count {
      font-size: 14px;
      color: #666;
    }
  
    .sort-options {
      display: flex;
      align-items: center;
      gap: 8px;
    }
  
    .sort-options label {
      font-size: 14px;
    }
  
    .sort-options select {
      padding: 8px 12px;
      border: 1px solid #ddd;
      border-radius: 6px;
      background-color: white;
    }
  
    .view-options {
      display: flex;
      align-items: center;
      gap: 4px;
    }
  
    .view-options button {
      display: flex;
      align-items: center;
      justify-content: center;
      width: 36px;
      height: 36px;
      background-color: white;
      border: 1px solid #ddd;
      border-radius: 6px;
      cursor: pointer;
      color: #666;
    }
  
    .view-options button.active {
      background-color: #f5f5f5;
      color: #333;
      border-color: #ccc;
    }
  
    /* Grille de produits */
    .products-grid {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
      gap: 20px;
    }
  
    .products-grid.list-view {
      grid-template-columns: 1fr;
    }
  
    /* Carte produit */
    .product-card {
      position: relative;
      border: 1px solid #eee;
      border-radius: 8px;
      overflow: hidden;
      background-color: white;
      transition: transform 0.2s, box-shadow 0.2s;
    }
  
    .product-card:hover {
      transform: translateY(-3px);
      box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
    }
  
    .list-view .product-card {
      display: flex;
      height: auto;
    }
  
    .product-badges {
      position: absolute;
      top: 10px;
      left: 10px;
      z-index: 1;
      display: flex;
      flex-direction: column;
      gap: 6px;
    }
  
    .badge {
      background-color: #ff5a5f;
      color: white;
      padding: 4px 8px;
      border-radius: 4px;
      font-size: 12px;
      font-weight: 600;
    }
  
    .product-image {
      position: relative;
      width: 100%;
      height: 200px;
    }
  
    .list-view .product-image {
      width: 180px;
      height: auto;
      flex-shrink: 0;
    }
  
    .product-image img {
      width: 100%;
      height: 100%;
      object-fit: cover;
    }
  
    .wishlist-button {
      position: absolute;
      top: 10px;
      right: 10px;
      width: 32px;
      height: 32px;
      background-color: white;
      border: none;
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      cursor: pointer;
      box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
      color: #666;
      transition: color 0.2s;
    }
  
    .wishlist-button:hover {
      color: #ff5a5f;
    }
  
    .product-info {
      padding: 15px;
    }
  
    .list-view .product-info {
      flex-grow: 1;
      display: flex;
      flex-direction: column;
    }
  
    .product-brand {
      font-size: 12px;
      color: #666;
      margin-bottom: 4px;
    }
  
    .product-name {
      font-size: 16px;
      font-weight: 600;
      margin: 0 0 8px;
      height: 40px;
      overflow: hidden;
      display: -webkit-box;
      -webkit-line-clamp: 2;
      -webkit-box-orient: vertical;
    }
  
    .list-view .product

    .list-view .product-name {
    height: auto;
    margin-bottom: 12px;
  }

  .product-pricing {
    display: flex;
    align-items: baseline;
    gap: 8px;
    margin-bottom: 8px;
  }

  .product-price {
    font-weight: 600;
    font-size: 18px;
  }

  .product-original-price {
    font-size: 14px;
    color: #999;
    text-decoration: line-through;
  }

  .product-discount {
    font-size: 14px;
    font-weight: 600;
    color: #ff5a5f;
  }

  .product-rating {
    display: flex;
    align-items: center;
    gap: 4px;
    margin-bottom: 8px;
  }

  .rating-stars {
    display: flex;
    align-items: center;
    gap: 1px;
    color: #ffb400;
  }

  .rating-stars span {
    margin-left: 4px;
    font-weight: 600;
    font-size: 14px;
  }

  .review-count {
    font-size: 12px;
    color: #666;
  }

  .product-features {
    margin: 12px 0;
  }
  
  .product-features ul {
    list-style-type: none;
    padding: 0;
    margin: 0;
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
  }
  
  .product-features li {
    font-size: 13px;
    background-color: #f5f5f5;
    padding: 4px 8px;
    border-radius: 4px;
    color: #555;
  }

  .product-availability {
    font-size: 14px;
    margin: 8px 0;
  }
  
  .in-stock {
    color: #4CAF50;
  }
  
  .out-of-stock {
    color: #f44336;
  }

  .product-colors {
    display: flex;
    align-items: center;
    gap: 4px;
    margin-bottom: 12px;
  }
  
  .color-dot {
    width: 14px;
    height: 14px;
    border-radius: 50%;
    background-color: var(--color, #888);
    border: 1px solid #ddd;
  }
  
  .color-count {
    font-size: 12px;
    color: #666;
    margin-left: 4px;
  }

  .product-actions {
    margin-top: auto;
    display: flex;
    gap: 8px;
  }
  
  .add-to-cart-btn {
    padding: 8px 16px;
    background-color: #ff5a5f;
    color: white;
    border: none;
    border-radius: 6px;
    font-weight: 600;
    cursor: pointer;
    transition: background-color 0.2s;
    flex-grow: 1;
  }
  
  .add-to-cart-btn:hover {
    background-color: #e04347;
  }
  
  .add-to-cart-btn:disabled {
    background-color: #cccccc;
    cursor: not-allowed;
  }
  
  .view-details-btn {
    padding: 8px 16px;
    background-color: white;
    color: #333;
    text-decoration: none;
    border: 1px solid #ddd;
    border-radius: 6px;
    font-weight: 600;
    text-align: center;
    flex-grow: 1;
  }
  
  .quick-view-overlay {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    align-items: center;
    justify-content: center;
    opacity: 0;
    transition: opacity 0.2s;
    pointer-events: none;
    text-decoration: none;
  }
  
  .quick-view-overlay span {
    padding: 10px 20px;
    background-color: white;
    color: #333;
    border-radius: 6px;
    font-weight: 600;
  }
  
  .product-card:hover .quick-view-overlay {
    opacity: 1;
    pointer-events: auto;
  }

  .no-products {
    padding: 40px;
    text-align: center;
    color: #666;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 16px;
  }
  
  .no-products svg {
    color: #999;
  }
  
  .no-products h3 {
    margin: 0;
    font-size: 18px;
  }
  
  .no-products p {
    margin: 0;
    font-size: 14px;
  }

  .pagination {
    margin-top: 40px;
    display: flex;
    justify-content: center;
    gap: 8px;
  }
  
  .pagination-arrow,
  .pagination-page {
    width: 36px;
    height: 36px;
    display: flex;
    align-items: center;
    justify-content: center;
    border: 1px solid #ddd;
    border-radius: 6px;
    background-color: white;
    cursor: pointer;
    transition: background-color 0.2s;
  }
  
  .pagination-page.active {
    background-color: #ff5a5f;
    color: white;
    border-color: #ff5a5f;
  }
  
  .pagination-arrow:disabled {
    color: #ccc;
    cursor: not-allowed;
  }
  
  .pagination-ellipsis {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 36px;
    height: 36px;
  }

  /* Responsive */
  @media (max-width: 900px) {
    .category-layout {
      flex-direction: column;
    }
    
    .filters-sidebar {
      width: 100%;
      position: sticky;
      top: 0;
      z-index: 10;
      background-color: white;
      padding: 10px;
      border-bottom: 1px solid #eee;
    }
    
    .products-grid {
      grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
    }
  }
  
  @media (max-width: 600px) {
    .products-grid {
      grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
    }
    
    .products-grid.list-view {
      grid-template-columns: 1fr;
    }
    
    .list-view .product-card {
      flex-direction: column;
    }
    
    .list-view .product-image {
      width: 100%;
      height: 200px;
    }
    
    .product-actions {
      flex-direction: column;
    }
  }
</style>