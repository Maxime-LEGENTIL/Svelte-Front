<script>
  import './MegaMenu.css';
  import { onMount } from 'svelte';

  let allCategories = [];
  let scrollPosition = 0;
  let containerRef;

  

  onMount(async () => {
  try {
    const res = await fetch('http://localhost:8000/api/categories?page[number]=1', {
      headers: {
        'Accept': 'application/ld+json',
        'Content-Type': 'application/ld+json'
      }
    });

    if (!res.ok) throw new Error('Erreur lors de la récupération des catégories');
    const json = await res.json();
    
    // Les catégories sont dans le tableau "member" et non "hydra:member" ou "data"
    allCategories = json.member ? json.member.map(category => ({
      id: category.id,
      name: category.name,
      icon: category.icon || '📦', // Valeur par défaut si l'icône est vide
      slug: category.slug
    })) : [];
    
    console.log('Catégories traitées:', allCategories);
  } catch (error) {
    console.error('Erreur API:', error);
  }
});

  function scrollLeft() {
    if (containerRef) containerRef.scrollBy({ left: -300, behavior: 'smooth' });
  }

  function scrollRight() {
    if (containerRef) containerRef.scrollBy({ left: 300, behavior: 'smooth' });
  }

  function navigateToCategory(categorySlug) {
    window.location.href = `/${categorySlug}`;
  }

  function onScroll() {
    if (containerRef) {
      scrollPosition = containerRef.scrollLeft;
    }
  }
</script>


<nav class="categories-megamenu">
  <div class="carousel-container">
    <button class="nav-button left" on:click={scrollLeft} style="visibility: {scrollPosition > 10 ? 'visible' : 'hidden'}">
      <span class="icon">◀</span>
    </button>

    <div class="categories-carousel" bind:this={containerRef} on:scroll={onScroll}>
      {#each allCategories as category}
        <a 
          href={`${category.slug}`}
          class="category-link" 
          on:click|preventDefault={() => navigateToCategory(category.slug)}
        >
          <div class="category-icon">{category.icon}</div>
          <div class="category-name">{category.name}</div>
        </a>
      {/each}
    </div>

    <button class="nav-button right" on:click={scrollRight}>
      <span class="icon">▶</span>
    </button>
  </div>
</nav>
