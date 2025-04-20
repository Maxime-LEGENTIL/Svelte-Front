<script>
    import { onMount } from 'svelte';
    import { fade, fly } from 'svelte/transition';
    import Banner from './Banner.svelte';
    import PopularCategories from './PopularCategories.svelte';

    // État du composant
    let searchTerm = '';
    let categories = [];
    let featuredProducts = [];
    let promotions = [];
    let isLoading = true;

    onMount(async () => {
      // Simulation de chargement des données depuis votre API Laravel
      // À remplacer par vos appels API réels
      setTimeout(() => {
        featuredProducts = [
          { id: 1, name: "Écouteurs sans fil", price: 89.99, rating: 4.8, reviews: 124, image: "https://www.backmarket.fr/cdn-cgi/image/format%3Dauto%2Cquality%3D75%2Cwidth%3D260/https://d2e6ccujb3mkqf.cloudfront.net/1e682053-382f-4cd5-bccf-63ed65067c30-1_b9017777-1390-4072-b865-a6a4ea41713a.jpg", discount: 15 },
          { id: 2, name: "Montre connectée", price: 159.99, rating: 4.5, reviews: 98, image: "https://www.backmarket.fr/cdn-cgi/image/format%3Dauto%2Cquality%3D75%2Cwidth%3D260/https://d2e6ccujb3mkqf.cloudfront.net/acd64e8b-426b-4468-b6c6-5fccd3a64d0a-1_83c94cf4-a9da-4a56-a417-9c8c801899fb.jpg" },
          { id: 3, name: "Sac à dos urbain", price: 49.99, rating: 4.7, reviews: 67, image: "https://www.backmarket.fr/cdn-cgi/image/format%3Dauto%2Cquality%3D75%2Cwidth%3D260/https://d2e6ccujb3mkqf.cloudfront.net/ba089b01-b907-40b3-8f71-6017c78aa316-1_53c4be26-80e4-4a2c-bc74-123a6f0f4450.jpg", discount: 20 },
          { id: 4, name: "Haut-parleur portable", price: 79.99, rating: 4.6, reviews: 142, image: "https://www.backmarket.fr/cdn-cgi/image/format%3Dauto%2Cquality%3D75%2Cwidth%3D260/https://d2e6ccujb3mkqf.cloudfront.net/5078a71a-4d70-46c9-9cc4-17a85a6d183f-1_294aa754-e3ba-4a06-bb5c-c4646cf30b2b.jpg" },
          { id: 5, name: "Clavier mécanique RGB", price: 129.90, rating: 4.9, reviews: 203, image: "https://www.backmarket.fr/cdn-cgi/image/format%3Dauto%2Cquality%3D75%2Cwidth%3D260/https://d2e6ccujb3mkqf.cloudfront.net/12df4f06-a16b-4156-a0f1-51f7614b687e-1_5fc3f3e4-5efb-48a0-9284-b4d01a26e379.jpg", discount: 10 },
          { id: 6, name: "Caméra de surveillance", price: 99.00, rating: 4.3, reviews: 88, image: "https://www.backmarket.fr/cdn-cgi/image/format%3Dauto%2Cquality%3D75%2Cwidth%3D260/https://d2e6ccujb3mkqf.cloudfront.net/943399d9-98dd-4c0f-89a9-cae385ef0a6a-1_e46dfa6b-cdfe-4cc8-a1b0-bb2b39fb357a.jpg" },
          { id: 7, name: "Casque audio Bluetooth", price: 149.00, rating: 4.7, reviews: 178, image: "https://www.backmarket.fr/cdn-cgi/image/format%3Dauto%2Cquality%3D75%2Cwidth%3D260/https://d2e6ccujb3mkqf.cloudfront.net/1e682053-382f-4cd5-bccf-63ed65067c30-1_b9017777-1390-4072-b865-a6a4ea41713a.jpg", discount: 25 },
          { id: 8, name: "Station de charge sans fil", price: 39.99, rating: 4.4, reviews: 56, image: "https://www.backmarket.fr/cdn-cgi/image/format%3Dauto%2Cquality%3D75%2Cwidth%3D260/https://d2e6ccujb3mkqf.cloudfront.net/1e682053-382f-4cd5-bccf-63ed65067c30-1_b9017777-1390-4072-b865-a6a4ea41713a.jpg" },
          { id: 9, name: "Support pour ordinateur", price: 29.99, rating: 4.6, reviews: 112, image: "https://www.backmarket.fr/cdn-cgi/image/format%3Dauto%2Cquality%3D75%2Cwidth%3D260/https://d2e6ccujb3mkqf.cloudfront.net/1e682053-382f-4cd5-bccf-63ed65067c30-1_b9017777-1390-4072-b865-a6a4ea41713a.jpg" },
          { id: 10, name: "Batterie externe 20 000mAh", price: 45.00, rating: 4.8, reviews: 134, image: "https://www.backmarket.fr/cdn-cgi/image/format%3Dauto%2Cquality%3D75%2Cwidth%3D260/https://d2e6ccujb3mkqf.cloudfront.net/1e682053-382f-4cd5-bccf-63ed65067c30-1_b9017777-1390-4072-b865-a6a4ea41713a.jpg", discount: 30 },
          { id: 11, name: "Lampe LED de bureau", price: 24.90, rating: 4.2, reviews: 39, image: "https://www.backmarket.fr/cdn-cgi/image/format%3Dauto%2Cquality%3D75%2Cwidth%3D260/https://d2e6ccujb3mkqf.cloudfront.net/1e682053-382f-4cd5-bccf-63ed65067c30-1_b9017777-1390-4072-b865-a6a4ea41713a.jpg" },
          { id: 12, name: "Souris ergonomique", price: 34.99, rating: 4.5, reviews: 71, image: "https://www.backmarket.fr/cdn-cgi/image/format%3Dauto%2Cquality%3D75%2Cwidth%3D260/https://d2e6ccujb3mkqf.cloudfront.net/1e682053-382f-4cd5-bccf-63ed65067c30-1_b9017777-1390-4072-b865-a6a4ea41713a.jpg" }
        ];

        
        promotions = [
          { id: 1, title: "Offres d'été", description: "Jusqu'à 40% de réduction", color: "promo-blue" },
          { id: 2, title: "Nouveautés", description: "Découvrez nos derniers produits", color: "promo-green" }
        ];
        
        isLoading = false;
      }, 1000);
    });

    // Gestionnaire d'événements
    function handleSearch(e) {
      e.preventDefault();
      // Logique pour rechercher des produits
      console.log("Recherche pour:", searchTerm);
    }

    // Helper pour les étoiles de notation
    function ratingStars(rating) {
      return Array(5).fill(0).map((_, i) => i < Math.floor(rating));
    }
</script>

<div class="container">
  {#if isLoading}
    <div class="loading-container" in:fade>
      <div class="loading-pulse">
        <div class="loading-circle"></div>
        <div class="loading-content">
          <div class="loading-line"></div>
          <div class="loading-line-short"></div>
        </div>
      </div>
    </div>
  {:else}
    <Banner></Banner>

        
    <PopularCategories></PopularCategories>
    
    <!-- Promotions -->
    <!--<section class="promotions-section" in:fly={{ y: 20, duration: 700, delay: 200 }}>
      <div class="promotions-grid">
        {#each promotions as promo (promo.id)}
          <div class="promo-card {promo.color}">
            <h3 class="promo-title">{promo.title}</h3>
            <p class="promo-description">{promo.description}</p>
            <button class="promo-link">
              En savoir plus <span class="arrow-icon">→</span>
            </button>
          </div>
        {/each}
      </div>
    </section>-->
    
    <!-- Produits en vedette -->
    <section class="products-section" in:fly={{ y: 20, duration: 800, delay: 300 }}>
      <div class="section-header">
        <h2>Produits populaires</h2>
        <button class="link-button">
          Voir plus <span class="arrow-icon">→</span>
        </button>
      </div>
      
      <div class="products-grid">
        {#each featuredProducts as product (product.id)}
          <div class="product-card">
            <div class="product-image-container">
              <img src={product.image} alt={product.name} class="product-image" />
              <button class="wishlist-button">
                <span class="heart-icon">♡</span>
              </button>
              {#if product.discount}
                <div class="discount-badge">
                  -{product.discount}%
                </div>
              {/if}
            </div>
            <div class="product-info">
              <h3 class="product-title">{product.name}</h3>
              <div class="product-rating">
                <!-- Étoiles de notation -->
                {#each ratingStars(product.rating) as isFilled}
                  <span class="star">{isFilled ? '★' : '☆'}</span>
                {/each}
                <span class="rating-score">{product.rating}</span>
                <span class="review-count">({product.reviews})</span>
              </div>
              <div class="product-footer">
                <div class="product-price">
                  {#if product.discount}
                    <span class="discounted-price">{(product.price * (1 - product.discount / 100)).toFixed(2)} €</span>
                    <span class="original-price">{product.price} €</span>
                  {:else}
                    <span class="price">{product.price} €</span>
                  {/if}
                </div>
                <button class="cart-button">
                  <span class="cart-icon" style="color: white; font-size: 24px;">+</span>
                </button>
              </div>
            </div>
          </div>
        {/each}
      </div>
    </section>
    
    <!-- Section avantages -->
    <section class="benefits-section" in:fly={{ y: 20, duration: 900, delay: 400 }}>
      <div class="benefits-grid">
        <div class="benefit-card">
          <div class="benefit-icon-container">
            <span class="benefit-icon">🏷️</span>
          </div>
          <h3 class="benefit-title">Meilleurs prix</h3>
          <p class="benefit-description">Nous garantissons les meilleurs prix pour tous nos produits.</p>
        </div>
        
        <div class="benefit-card">
          <div class="benefit-icon-container">
            <span class="benefit-icon">🚚</span>
          </div>
          <h3 class="benefit-title">Livraison rapide</h3>
          <p class="benefit-description">Recevez vos commandes en un temps record avec notre service de livraison express.</p>
        </div>
        
        <div class="benefit-card">
          <div class="benefit-icon-container">
            <span class="benefit-icon">⭐</span>
          </div>
          <h3 class="benefit-title">Satisfaction garantie</h3>
          <p class="benefit-description">Satisfait ou remboursé sous 30 jours pour tous vos achats.</p>
        </div>
      </div>
    </section>
    
    <!-- CTA Newsletter -->
    <section class="newsletter-section" in:fly={{ y: 20, duration: 1000, delay: 500 }}>
      <div class="newsletter-container">
        <h2 class="newsletter-title">Rejoignez notre newsletter</h2>
        <p class="newsletter-description">Recevez les dernières offres et nouveautés directement dans votre boîte mail</p>
        
        <form class="newsletter-form">
          <input 
            type="email" 
            placeholder="Votre adresse email" 
            class="newsletter-input"
          />
          <button class="newsletter-button">
            S'abonner
          </button>
        </form>
      </div>
    </section>
  {/if}
</div>

<style>
  /* Styles généraux */
  .container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 0 20px;
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen-Sans, Ubuntu, Cantarell, 'Helvetica Neue', sans-serif;

  }

  section {
    margin-bottom: 60px;
  }

  /* État de chargement */
  .loading-container {
    display: flex;
    align-items: center;
    justify-content: center;
    min-height: 80vh;
  }

  .loading-pulse {
    display: flex;
    gap: 16px;
  }

  .loading-circle {
    width: 48px;
    height: 48px;
    border-radius: 50%;
    background-color: #e2e8f0;
  }

  .loading-content {
    flex: 1;
    padding: 4px 0;
  }

  .loading-line, .loading-line-short {
    height: 16px;
    background-color: #e2e8f0;
    border-radius: 4px;
    margin-bottom: 16px;
  }

  .loading-line-short {
    width: 80%;
  }

  @keyframes pulse {
    0%, 100% {
      opacity: 1;
    }
    50% {
      opacity: 0.5;
    }
  }

  .loading-pulse {
    animation: pulse 2s cubic-bezier(0.4, 0, 0.6, 1) infinite;
  }

  /* Section Headers */
  .section-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 24px;
  }

  .section-header h2 {
    font-size: 1.5rem;
    font-weight: 700;
  }

  .link-button {
    color: #ff5a5f;
    background: none;
    border: none;
    font-size: 1rem;
    cursor: pointer;
    display: flex;
    align-items: center;
    transition: color 0.2s;
  }

  .link-button:hover {
    color: #ff5a5f;
  }

  .arrow-icon {
    margin-left: 4px;
  }

  /* Products Section */
  .products-grid {
    display: grid;
    grid-template-columns: 1fr;
    gap: 24px;
  }

  @media (min-width: 640px) {
    .products-grid {
      grid-template-columns: repeat(2, 1fr);
    }
  }

  @media (min-width: 1024px) {
    .products-grid {
      grid-template-columns: repeat(4, 1fr);
    }
  }

  .product-card {
    background-color: white;
    border-radius: 12px;
    overflow: hidden;
    box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
    transition: box-shadow 0.2s;
    border: 1px solid transparent;
  }

  .product-card:hover {
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    border: 1px solid #ff5a5f;
  }

  .product-image-container {
    position: relative;
    display: flex;
    justify-content: center;
  }

  .product-image {
    width: 80%;
    height: 192px;
    object-fit: cover;
    margin-top: 25px;
  }

  .wishlist-button {
    position: absolute;
    top: 12px;
    right: 12px;
    background-color: white;
    border: none;
    border-radius: 50%;
    width: 32px;
    height: 32px;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
  }

  .heart-icon {
    color: #6b7280;
    font-size: 1.25rem;
  }

  .discount-badge {
    position: absolute;
    top: 12px;
    left: 12px;
    background-color: #ef4444;
    color: white;
    font-size: 0.75rem;
    font-weight: 700;
    padding: 4px 8px;
    border-radius: 4px;
  }

  .product-info {
    padding: 16px;
  }

  .product-title {
    font-weight: 500;
    margin-bottom: 4px;
  }

  .product-rating {
    display: flex;
    align-items: center;
    margin-bottom: 8px;
    gap: 4px;
  }

  .star {
    color: #fbbf24;
  }

  .rating-score {
    font-size: 0.875rem;
    font-weight: 500;
  }

  .review-count {
    font-size: 0.875rem;
    color: #6b7280;
  }

  .product-footer {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .price, .discounted-price {
    font-weight: 700;
  }

  .original-price {
    font-size: 0.875rem;
    color: #6b7280;
    text-decoration: line-through;
    margin-left: 8px;
  }

  .cart-button {
    background-color: #FF5A5F;
    color: white;
    border: none;
    border-radius: 50%;
    width: 32px;
    height: 32px;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    transition: background-color 0.2s;
    box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
  }

  .cart-button:hover {
    background-color: #FF5A5F;
  }

  .cart-icon {
    font-size: 1rem;
  }

  /* Benefits Section */
  .benefits-grid {
    display: grid;
    grid-template-columns: 1fr;
    gap: 24px;
  }

  @media (min-width: 768px) {
    .benefits-grid {
      grid-template-columns: repeat(3, 1fr);
    }
  }

  .benefit-card {
    display: flex;
    flex-direction: column;
    align-items: center;
    text-align: center;
    padding: 24px;
    background-color: #f9fafb;
    border-radius: 12px;
  }

  .benefit-icon-container {
    background-color: #dbeafe;
    color: #ff5a5f;
    width: 48px;
    height: 48px;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    margin-bottom: 16px;
  }

  .benefit-icon {
    font-size: 1.5rem;
  }

  .benefit-title {
    font-weight: 700;
    margin-bottom: 8px;
  }

  .benefit-description {
    color: #6b7280;
    margin: 0;
  }

  /* Newsletter Section */
  .newsletter-container {
    background-color: #f3f4f6;
    border-radius: 12px;
    padding: 32px;
    text-align: center;
  }

  .newsletter-title {
    font-size: 1.5rem;
    font-weight: 700;
    margin-bottom: 12px;
  }

  .newsletter-description {
    margin-bottom: 24px;
    max-width: 500px;
    margin-left: auto;
    margin-right: auto;
  }

  .newsletter-form {
    display: flex;
    flex-direction: column;
    gap: 8px;
    max-width: 400px;
    margin: 0 auto;
  }

  @media (min-width: 640px) {
    .newsletter-form {
      flex-direction: row;
    }
  }

  .newsletter-input {
    flex-grow: 1;
    padding: 12px;
    border-radius: 8px;
    border: 1px solid #e5e7eb;
  }

  .newsletter-button {
    background-color: #ff5a5f;
    color: white;
    border: none;
    border-radius: 8px;
    padding: 12px 24px;
    font-weight: 500;
    cursor: pointer;
    transition: background-color 0.2s;
  }

  .newsletter-button:hover {
    background-color: #ff5a5f;
  }
</style>