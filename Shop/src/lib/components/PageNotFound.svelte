<section class="error-page">
  <div class="content">
    <h1 class="title">Oups, cette page n’existe pas...</h1>
    <p class="message">Pas de panique, on vous ramène à bon port ✨</p>

    <a href="/" class="home-link">
      Retour à l’accueil
    </a>
  </div>
</section>

<style>
  .error-page {
    min-height: 80vh;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 2rem;
    text-align: center;
    background-color: #f7f7f7;
    font-family: system-ui, sans-serif;
  }

  .content {
    max-width: 500px;
  }

  .title {
    font-size: 1.8rem;
    font-weight: 700;
    color: #333;
    margin-bottom: 0.5rem;
  }

  .message {
    font-size: 1rem;
    color: #555;
    margin-bottom: 2rem;
  }

  .home-link {
    display: inline-block;
    background-color: #FF5A5F;
    color: white;
    text-decoration: none;
    padding: 0.75rem 1.5rem;
    font-size: 1rem;
    border-radius: 999px;
    transition: background-color 0.2s;
  }

  .home-link:hover {
    background-color: #e14c50;
  }
</style>
