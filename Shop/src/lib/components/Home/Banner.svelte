<script>
    import { fly } from 'svelte/transition';
</script>

<div class="hero-section" in:fly={{ y: 20, duration: 500 }}>
    <a href="/category" class="hero-link">
        <img
            src="https://images.ctfassets.net/mmeshd7gafk1/4ckNwYhbq9viSKvv1lK7W7/8181c490b8f45f3e01ef29977f0f318e/DN2025W1_HP_Banners_Desktop_Laptop_FR.jpg"
            alt="Bannière promotionnelle présentant un PC portable pas cher"
            class="hero-image"
        />
    </a>
</div>

<style>
    /* Hero Section */
    .hero-section {
        overflow: hidden;
        margin-top: 40px;
        margin-bottom: 40px;
    }

    .hero-link {
        display: block;
    }

    .hero-image {
        width: 100%;
        height: auto;
        display: block;
        cursor: pointer;
        transition: opacity 0.3s ease;
    }

    .hero-image:hover {
        opacity: 0.9;
    }
</style>