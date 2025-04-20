<script>
    import { onMount } from 'svelte';
    import { fade, fly } from 'svelte/transition';

    // État du composant
    let categories = [];
    
    onMount(async () => {
        categories = await new Promise((resolve) => {
            setTimeout(() => {
                resolve([
                    { id: 1, name: "Électronique", icon: "💻", count: 120 },
                    { id: 2, name: "Mode", icon: "👕", count: 85 },
                    { id: 3, name: "Maison", icon: "🏠", count: 64 },
                    { id: 4, name: "Sports", icon: "⚽", count: 47 }
                ]);
            }, 1000);
        });
    });

</script>

<!-- Catégories populaires -->
<section class="categories-section" in:fly={{ y: 20, duration: 600, delay: 100 }}>
    <div class="section-header">
        <h2>Recommandé pour vous</h2>
        <button class="link-button">
        Toutes les catégories <span class="arrow-icon">→</span>
        </button>
    </div>
    
    <div class="categories-grid">
        {#each categories as category (category.id)}
        <div class="category-card">
            <div class="category-icon">{category.icon}</div>
            <h3 class="category-title">{category.name}</h3>
            <p class="category-count">{category.count} produits</p>
        </div>
        {/each}
    </div>
</section>

<style>
    h2, h3 {
        margin: 0;
        font-family: 'Segoe UI', Arial, sans-serif;
    }
    
    .section-header {
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 24px;
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

    /* Categories Section */
    .categories-grid {
        display: grid;
        grid-template-columns: repeat(2, 1fr);
        gap: 16px;
    }

    @media (min-width: 768px) {
        .categories-grid {
        grid-template-columns: repeat(4, 1fr);
        }
    }

    .category-card {
        background-color: white;
        border-radius: 12px;
        padding: 24px;
        display: flex;
        flex-direction: column;
        align-items: center;
        text-align: center;
        box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
        transition: box-shadow 0.2s;
        cursor: pointer;
        border: 1px solid transparent;

    }

    .category-card:hover {
        box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        border: 1px solid #ff5a5f;
    }

    .category-icon {
        font-size: 2rem;
        margin-bottom: 8px;
    }

    .category-title {
        font-weight: 500;
        margin-bottom: 4px;
    }

    .category-count {
        font-size: 0.875rem;
        color: #6b7280;
        margin: 0;
    }
</style>