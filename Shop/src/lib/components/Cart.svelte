<script>
  import { cart } from '$lib/stores/cartStore.js';
  import { get } from 'svelte/store';

  let items = [];
  $: items = $cart;

  $: isEmpty = items.length === 0;
  $: totalPrice = items.reduce((sum, item) => sum + item.price * item.quantity, 0).toFixed(2);

  function removeItem(id) {
    cart.update(items => items.filter(item => item.id !== id));
  }

  function goToCheckout() {
    window.location.href = '/checkout';
  }

function addToCart() {
  cart.update(items => {
    const existing = items.find(i => i.id === 1);
    if (existing) {
      return items.map(i =>
        i.id === 1 ? { ...i, quantity: i.quantity + 1 } : i
      );
    }
    return [
      ...items,
      {
        id: 1,
        name: 'iPhone 12',
        price: 499.99,
        quantity: 1,
        image: 'https://www.backmarket.fr/cdn-cgi/image/format%3Dauto%2Cquality%3D75%2Cwidth%3D260/https://d2e6ccujb3mkqf.cloudfront.net/ef5660d2-6883-4b81-b47d-86e5720687ef-1_72ddd233-c9b6-4f10-a739-447c440b6357.jpg'
      }
    ];
  });
}
</script>

  
  <section class="cart-section">
    <h2 class="cart-title">Mon panier</h2>
  
    {#if isEmpty}
      <div class="cart-empty">
        <img src="https://cdn-icons-png.flaticon.com/512/2038/2038854.png" alt="Panier vide" />
        <p>Votre panier est vide pour le moment.</p>
        <a href="/category" class="button">Explorer les produits</a>
        <button on:click={addToCart}>Ajouter au panier</button>

      </div>
    {:else}
      <ul class="cart-items">
        {#each items as item}
          <li class="cart-item">
            <img src={item.image} alt={item.name} class="item-img" />
            <div class="item-details">
              <h3>{item.name}</h3>
              <p>{item.price.toFixed(2)} € × {item.quantity}</p>
              <button class="remove-button" on:click={() => removeItem(item.id)}>Supprimer</button>
            </div>
          </li>
        {/each}
      </ul>
  
      <div class="cart-summary">
        <p class="total-label">Total :</p>
        <p class="total-price">{totalPrice} €</p>
      </div>
  
      <button class="checkout-button" on:click={goToCheckout}>
        Passer au paiement
      </button>
    {/if}
  </section>
  
  <style>
    .cart-section {
      max-width: 800px;
      margin: 2rem auto;
      padding: 1.5rem;
      background: white;
      border-radius: 16px;
      box-shadow: 0 6px 20px rgba(0,0,0,0.05);
      font-family: 'Inter', sans-serif;
    }
  
    .cart-title {
      font-size: 1.5rem;
      font-weight: 600;
      margin-bottom: 1.5rem;
    }
  
    .cart-empty {
      text-align: center;
      color: #555;
    }
  
    .cart-empty img {
      width: 120px;
      margin-bottom: 1rem;
    }
  
    .cart-items {
      list-style: none;
      padding: 0;
      margin: 0 0 1.5rem;
    }
  
    .cart-item {
      display: flex;
      align-items: center;
      gap: 1rem;
      border-bottom: 1px solid #eee;
      padding: 1rem 0;
    }
  
    .item-img {
      width: 80px;
      height: 80px;
      object-fit: cover;
      border-radius: 8px;
    }
  
    .item-details {
      flex-grow: 1;
    }
  
    .item-details h3 {
      margin: 0 0 0.25rem;
      font-size: 1rem;
    }
  
    .remove-button {
      background: none;
      border: none;
      color: #ff5a5f;
      font-size: 0.9rem;
      cursor: pointer;
      padding: 0;
    }
  
    .remove-button:hover {
      text-decoration: underline;
    }
  
    .cart-summary {
      display: flex;
      justify-content: space-between;
      font-size: 1.2rem;
      font-weight: 500;
      margin-bottom: 1rem;
    }
  
    .checkout-button,
    .button {
      background-color: #ff5a5f;
      color: white;
      padding: 0.75rem 1.5rem;
      border: none;
      border-radius: 12px;
      font-size: 1rem;
      cursor: pointer;
      text-align: center;
      display: inline-block;
      transition: background 0.2s ease;
    }
  
    .checkout-button:hover,
    .button:hover {
      background-color: #e04850;
    }
  
    a.button {
      text-decoration: none;
    }
  </style>
  