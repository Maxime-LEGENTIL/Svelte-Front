<script>
  import { onMount } from 'svelte';

  let product = null;
  let selectedImageIndex = 0;
  let quantity = 1;
  let loading = true;
  let error = null;

  onMount(async () => {
    try {
      const res = await fetch('http://localhost:8000/api/products/1');

      if (!res.ok) {
        throw new Error(`Erreur API: ${res.status}`);
      }

      const data = await res.json();

      product = {
        ...data,
        // Données mock/fallback
        images: [
          "https://www.cdiscount.com/pdt2/4/b/6/1/700x700/celedgam43ql24b6/rw/tv-qled-uhd-4k-gaming-continental-edison-43.jpg",
          "https://www.cdiscount.com/pdt2/4/b/6/5/700x700/celedgam43ql24b6/rw/tv-qled-uhd-4k-gaming-continental-edison-43.jpg",
          "https://www.cdiscount.com/pdt2/4/b/6/4/700x700/celedgam43ql24b6/rw/tv-qled-uhd-4k-gaming-continental-edison-43.jpg"
        ],
        rating: 4.7,
        reviewCount: 142,
        originalPrice: data.price * 1.1,
        badges: ["Nouveau"],
        features: ["Confortable", "Design moderne", "Léger"],
        seller: {
          name: "Nom du vendeur",
          rating: 4.8,
          verified: true
        }
      };
    } catch (err) {
      error = err.message;
    } finally {
      loading = false;
    }
  });

  function addToCart() {
    alert(`${quantity} ${product.name} ajouté(s) au panier`);
  }

  function addToWishlist() {
    alert(`${product.name} ajouté aux favoris`);
  }

  function formatPrice(price) {
    return new Intl.NumberFormat('fr-FR', {
      style: 'currency',
      currency: 'EUR'
    }).format(price);
  }
</script>

{#if loading}
  <p>Chargement en cours...</p>
{:else if error}
  <p style="color: red;">Erreur : {error}</p>
{:else if product}
  <div class="product-card">
    <!-- Galerie -->
    <div class="image-gallery">
      <div class="main-image">
        <img src={product.images[selectedImageIndex]} alt={product.name} />

        {#if product.badges?.length}
          <div class="badges">
            {#each product.badges as badge}
              <span class="badge">{badge}</span>
            {/each}
          </div>
        {/if}

        <button class="wishlist-button" on:click={addToWishlist} aria-label="Ajouter aux favoris">
          <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" fill="none" stroke="currentColor" stroke-width="2">
            <path d="M20.84 4.61a5.5 5.5 0 0 0-7.78 0L12 5.67l-1.06-1.06a5.5 5.5 0 0 0-7.78 7.78L12 21.23l7.78-7.78a5.5 5.5 0 0 0 0-7.78z"/>
          </svg>
        </button>
      </div>

      <div class="thumbnails">
        {#each product.images as image, index}
          <button
            class="thumbnail {selectedImageIndex === index ? 'active' : ''}"
            on:click={() => selectedImageIndex = index}
            aria-label={`Image ${index + 1}`}
          >
            <img src={image} alt={`${product.name} - Vue ${index + 1}`} />
          </button>
        {/each}
      </div>
    </div>

    <!-- Infos produit -->
    <div class="product-info">
      <div class="header">
        <h1>{product.name}</h1>

        <div class="rating">
          <span class="stars">
            {#each Array(5) as _, i}
              <svg
                xmlns="http://www.w3.org/2000/svg"
                width="16"
                height="16"
                viewBox="0 0 24 24"
                fill={i < Math.floor(product.rating) ? "currentColor" : "none"}
                stroke="currentColor"
                stroke-width="2"
                class="star"
              >
                <polygon points="12 2 15.09 8.26 22 9.27 17 14.14 18.18 21.02 12 17.77 5.82 21.02 7 14.14 2 9.27 8.91 8.26 12 2" />
              </svg>
            {/each}
            <span class="rating-value">{product.rating}</span>
          </span>
          <span class="reviews">({product.reviewCount} avis)</span>
        </div>
      </div>

      <div class="pricing">
        <div class="current-price">{formatPrice(product.price)}</div>
        {#if product.originalPrice > product.price}
          <div class="original-price">{formatPrice(product.originalPrice)}</div>
          <div class="discount">-{Math.round((1 - product.price / product.originalPrice) * 100)}%</div>
        {/if}
      </div>

      <div class="seller">
        <span class="seller-name">
          Vendu par {product.seller.name}
          {#if product.seller.verified}
            <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="#4CAF50" stroke="#fff" stroke-width="1" class="verified-icon">
              <path d="M9 16.17L4.83 12l-1.42 1.41L9 19 21 7l-1.41-1.41z" />
            </svg>
          {/if}
        </span>
        <span class="seller-rating">{product.seller.rating} ★</span>
      </div>

      <div class="description">
        <p>{product.description}</p>
      </div>

      {#if product.features?.length}
        <div class="features">
          <h3>Caractéristiques</h3>
          <ul>
            {#each product.features as feature}
              <li>{feature}</li>
            {/each}
          </ul>
        </div>
      {/if}

      <div class="stock {product.stock < 5 ? 'low-stock' : ''}">
        {#if product.stock > 0}
          {#if product.stock < 5}
            <span>Plus que {product.stock} en stock !</span>
          {:else}
            <span>En stock ({product.stock})</span>
          {/if}
        {:else}
          <span class="out-of-stock">Rupture de stock</span>
        {/if}
      </div>

      <div class="actions">
        <div class="quantity">
          <button on:click={() => quantity = Math.max(1, quantity - 1)}>-</button>
          <input type="number" bind:value={quantity} min="1" max={product.stock} />
          <button on:click={() => quantity = Math.min(product.stock, quantity + 1)}>+</button>
        </div>

        <button class="add-to-cart" on:click={addToCart} disabled={product.stock === 0}>
          {product.stock === 0 ? 'Indisponible' : 'Ajouter au panier'}
        </button>
      </div>
    </div>
  </div>
{/if}


<style>
  .product-card {
    display: flex;
    flex-direction: column;
    max-width: 1200px;
    margin: 0 auto;
    padding: 20px;
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  }

  @media (min-width: 768px) {
    .product-card {
      flex-direction: row;
      gap: 40px;
    }
  }

  .image-gallery {
    flex: 1;
    position: relative;
  }

  .main-image {
    position: relative;
    border-radius: 12px;
    overflow: hidden;
    margin-bottom: 12px;
  }

  .main-image img {
    width: 100%;
    height: auto;
    object-fit: cover;
    aspect-ratio: 4/3;
    display: block;
  }

  .thumbnails {
    display: flex;
    gap: 8px;
    overflow-x: auto;
    padding-bottom: 8px;
  }

  .thumbnail {
    width: 80px;
    height: 60px;
    border-radius: 8px;
    overflow: hidden;
    cursor: pointer;
    padding: 0;
    border: 2px solid transparent;
    background: none;
  }

  .thumbnail.active {
    border-color: #ff5a5f;
  }

  .thumbnail img {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }

  .badges {
    position: absolute;
    top: 12px;
    left: 12px;
    display: flex;
    gap: 8px;
  }

  .badge {
    background-color: #ff5a5f;
    color: white;
    padding: 4px 12px;
    border-radius: 20px;
    font-size: 12px;
    font-weight: bold;
  }

  .wishlist-button {
    position: absolute;
    top: 12px;
    right: 12px;
    background-color: white;
    border: none;
    border-radius: 50%;
    width: 40px;
    height: 40px;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.15);
  }

  .wishlist-button svg {
    color: #ff5a5f;
  }

  .product-info {
    flex: 1;
  }

  .header {
    margin-bottom: 16px;
  }

  h1 {
    font-size: 24px;
    font-weight: 600;
    margin: 0 0 12px;
  }

  .rating {
    display: flex;
    align-items: center;
    gap: 8px;
  }

  .stars {
    display: flex;
    align-items: center;
  }

  .star {
    color: #ff5a5f;
  }

  .rating-value {
    margin-left: 6px;
    font-weight: 600;
  }

  .reviews {
    color: #666;
  }

  .pricing {
    display: flex;
    align-items: baseline;
    gap: 12px;
    margin-bottom: 16px;
  }

  .current-price {
    font-size: 24px;
    font-weight: 600;
    color: #333;
  }

  .original-price {
    font-size: 18px;
    color: #999;
    text-decoration: line-through;
  }

  .discount {
    font-size: 16px;
    font-weight: 600;
    color: #ff5a5f;
  }

  .seller {
    display: flex;
    justify-content: space-between;
    margin-bottom: 20px;
    padding: 12px;
    background-color: #f8f8f8;
    border-radius: 8px;
  }

  .seller-name {
    display: flex;
    align-items: center;
    gap: 6px;
  }

  .verified-icon {
    margin-left: 4px;
  }

  .description {
    margin-bottom: 20px;
    line-height: 1.5;
  }

  .features {
    margin-bottom: 20px;
  }

  .features h3 {
    margin-bottom: 12px;
    font-size: 18px;
    font-weight: 600;
  }

  .features ul {
    list-style-type: none;
    padding: 0;
    display: flex;
    flex-wrap: wrap;
    gap: 12px;
  }

  .features li {
    background-color: #eef4fd;
    padding: 8px 16px;
    border-radius: 20px;
    font-size: 14px;
    display: flex;
    align-items: center;
  }

  .features li::before {
    content: "✓";
    margin-right: 6px;
    color: #4CAF50;
    font-weight: bold;
  }

  .stock {
    margin-bottom: 20px;
    padding: 12px;
    border-radius: 8px;
    background-color: #f0f9f0;
    color: #2e7d32;
    font-weight: 500;
  }

  .low-stock {
    background-color: #fff8e1;
    color: #ff8f00;
  }

  .out-of-stock {
    color: #d32f2f;
  }

  .actions {
    display: flex;
    gap: 16px;
    margin-top: 24px;
  }

  .quantity {
    display: flex;
    align-items: center;
    border: 1px solid #ddd;
    border-radius: 8px;
    overflow: hidden;
  }

  .quantity button {
    width: 40px;
    height: 48px;
    background-color: #f8f8f8;
    border: none;
    font-size: 20px;
    cursor: pointer;
  }

  .quantity input {
    width: 50px;
    height: 48px;
    border: none;
    border-left: 1px solid #ddd;
    border-right: 1px solid #ddd;
    text-align: center;
    font-size: 16px;
  }

  .add-to-cart {
    flex-grow: 1;
    height: 48px;
    background-color: #ff5a5f;
    color: white;
    border: none;
    border-radius: 8px;
    font-size: 16px;
    font-weight: 600;
    cursor: pointer;
    transition: background-color 0.2s;
  }

  .add-to-cart:hover {
    background-color: #e74c51;
  }

  .add-to-cart:disabled {
    background-color: #ccc;
    cursor: not-allowed;
  }

  /* Retirer les flèches natives à l'élément input number */
  /* Firefox */
  input[type=number] {
      -moz-appearance: textfield;
  }
  
  /* Chrome */
  input::-webkit-inner-spin-button,
  input::-webkit-outer-spin-button { 
    -webkit-appearance: none;
    margin:0;
  }
  
  /* Opéra*/
  input::-o-inner-spin-button,
  input::-o-outer-spin-button { 
    -o-appearance: none;
    margin:0
  }
</style>