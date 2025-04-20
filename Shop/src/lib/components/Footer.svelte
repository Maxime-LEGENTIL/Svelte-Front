<!-- Footer.svelte -->
<script>
    // Année courante pour le copyright
    const currentYear = new Date().getFullYear();
  </script>
  
  <footer class="footer">
    <div class="footer-container">
      <!-- Section principale -->
      <div class="footer-sections">
        <!-- Colonne 1 -->
        <div class="footer-section">
          <h3>À propos</h3>
          <ul>
            <li><a href="/about">Notre histoire</a></li>
            <li><a href="/team">Notre équipe</a></li>
            <li><a href="/careers">Rejoignez-nous</a></li>
            <li><a href="/press">Presse</a></li>
            <li><a href="/blog">Blog</a></li>
          </ul>
        </div>
  
        <!-- Colonne 2 -->
        <div class="footer-section">
          <h3>Support</h3>
          <ul>
            <li><a href="/help">Centre d'aide</a></li>
            <li><a href="/faq">FAQ</a></li>
            <li><a href="/safety">Sécurité</a></li>
            <li><a href="/contact">Contactez-nous</a></li>
          </ul>
        </div>
  
        <!-- Colonne 3 -->
        <div class="footer-section">
          <h3>Informations légales</h3>
          <ul>
            <li><a href="/terms">Conditions d'utilisation</a></li>
            <li><a href="/privacy">Politique de confidentialité</a></li>
            <li><a href="/cookies">Gestion des cookies</a></li>
            <li><a href="/accessibility">Accessibilité</a></li>
          </ul>
        </div>
        
        <!-- Colonne 4 -->
        <div class="footer-section">
          <h3>Suivez-nous</h3>
          <div class="social-links">
            <a href="https://facebook.com" aria-label="Facebook">
              <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <path d="M18 2h-3a5 5 0 0 0-5 5v3H7v4h3v8h4v-8h3l1-4h-4V7a1 1 0 0 1 1-1h3z"></path>
              </svg>
            </a>
            <a href="https://twitter.com" aria-label="Twitter">
              <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <path d="M22 4s-.7 2.1-2 3.4c1.6 10-9.4 17.3-18 11.6 2.2.1 4.4-.6 6-2C3 15.5.5 9.6 3 5c2.2 2.6 5.6 4.1 9 4-.9-4.2 4-6.6 7-3.8 1.1 0 3-1.2 3-1.2z"></path>
              </svg>
            </a>
            <a href="https://instagram.com" aria-label="Instagram">
              <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <rect x="2" y="2" width="20" height="20" rx="5" ry="5"></rect>
                <path d="M16 11.37A4 4 0 1 1 12.63 8 4 4 0 0 1 16 11.37z"></path>
                <line x1="17.5" y1="6.5" x2="17.5" y2="6.5"></line>
              </svg>
            </a>
            <a href="https://linkedin.com" aria-label="LinkedIn">
              <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <path d="M16 8a6 6 0 0 1 6 6v7h-4v-7a2 2 0 0 0-2-2 2 2 0 0 0-2 2v7h-4v-7a6 6 0 0 1 6-6z"></path>
                <rect x="2" y="9" width="4" height="12"></rect>
                <circle cx="4" cy="4" r="2"></circle>
              </svg>
            </a>
          </div>
        </div>
      </div>
      
      <!-- Barre inférieure -->
      <div class="footer-bottom">
        <div class="footer-logo">
          <a href="/">
            <!--<img src="/logo.svg" alt="Logo" width="120" height="40" />-->
            AppMarket
          </a>
        </div>
        <div class="footer-copyright">
          © {currentYear} AppMarket. Tous droits réservés.
        </div>
        <div class="footer-locale">
          <select aria-label="Changer de langue">
            <option value="fr">Français</option>
            <option value="en">English</option>
            <option value="es">Español</option>
            <option value="de">Deutsch</option>
          </select>
          <select aria-label="Changer de devise">
            <option value="eur">EUR (€)</option>
            <option value="usd">USD ($)</option>
            <option value="gbp">GBP (£)</option>
          </select>
        </div>
      </div>
    </div>
  </footer>
  
  <style>
    .footer {
      background-color: #f7f7f7;
      color: #484848;
      padding: 40px 0 20px;
      border-top: 1px solid #ddd;
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
    }
  
    .footer-container {
      max-width: 1200px;
      margin: 0 auto;
      padding: 0 24px;
    }
  
    .footer-sections {
      display: flex;
      flex-wrap: wrap;
      justify-content: space-between;
      margin-bottom: 40px;
    }
  
    .footer-section {
      flex: 1 1 200px;
      margin-bottom: 24px;
      min-width: 150px;
    }
  
    .footer-section h3 {
      font-size: 16px;
      font-weight: 600;
      margin-bottom: 16px;
      color: #222;
    }
  
    .footer-section ul {
      list-style: none;
      padding: 0;
      margin: 0;
    }
  
    .footer-section ul li {
      margin-bottom: 12px;
    }
  
    .footer-section ul li a {
      color: #717171;
      text-decoration: none;
      font-size: 14px;
      transition: color 0.2s;
    }
  
    .footer-section ul li a:hover {
      color: #ff5a5f;
      text-decoration: underline;
    }
  
    .social-links {
      display: flex;
      gap: 16px;
    }
  
    .social-links a {
      color: #717171;
      transition: color 0.2s;
    }
  
    .social-links a:hover {
      color: #ff5a5f;
    }
  
    .footer-bottom {
      display: flex;
      flex-wrap: wrap;
      justify-content: space-between;
      align-items: center;
      padding-top: 20px;
      border-top: 1px solid #ddd;
    }
  
    .footer-logo {
      margin-bottom: 16px;
    }
  
    .footer-copyright {
      font-size: 14px;
      color: #717171;
      margin-bottom: 16px;
    }
  
    .footer-locale {
      display: flex;
      gap: 12px;
      margin-bottom: 16px;
    }
  
    .footer-locale select {
      padding: 8px 12px;
      border: 1px solid #ddd;
      border-radius: 4px;
      background-color: white;
      font-size: 14px;
      color: #222;
      cursor: pointer;
    }
  
    /* Responsive */
    @media (max-width: 768px) {
      .footer-sections {
        flex-direction: column;
      }
      
      .footer-section {
        margin-bottom: 32px;
      }
      
      .footer-bottom {
        flex-direction: column;
        align-items: flex-start;
      }
      
      .footer-locale {
        flex-direction: column;
        width: 100%;
      }
      
      .footer-locale select {
        width: 100%;
      }
    }
  </style>