<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <meta name="theme-color" content="#ea580c" />
  <title>Samuel Food - Pedidos de Comida</title>
  <meta name="description" content="Plataforma moderna de pedidos de hamburguesas, perros, salchipapas y sandwiches con asistente inteligente." />
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&family=Space+Grotesk:wght@700&display=swap" rel="stylesheet" />
  <style>
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    :root {
      --bg: #0f0b09;
      --fg: #f5efe6;
      --card: #1a1410;
      --card-fg: #f5efe6;
      --primary: #ea580c;
      --primary-fg: #ffffff;
      --secondary: #261e17;
      --secondary-fg: #e8ddd1;
      --muted: #2b2219;
      --muted-fg: #887a6d;
      --accent: #dc2626;
      --accent-fg: #ffffff;
      --border: #2b2219;
      --ring: #ea580c;
      --radius: 0.75rem;
    }

    html { scroll-behavior: smooth; }

    body {
      font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
      background: var(--bg);
      color: var(--fg);
      line-height: 1.5;
      -webkit-font-smoothing: antialiased;
    }

    ::-webkit-scrollbar { width: 8px; }
    ::-webkit-scrollbar-track { background: #110e0a; }
    ::-webkit-scrollbar-thumb { background: var(--primary); border-radius: 4px; }
    ::-webkit-scrollbar-thumb:hover { background: #f97316; }

    a { color: inherit; text-decoration: none; }
    button { cursor: pointer; font-family: inherit; }
    img { max-width: 100%; display: block; }

    /* ===== ANIMATIONS ===== */
    @keyframes fadeInUp {
      from { opacity: 0; transform: translateY(20px); }
      to { opacity: 1; transform: translateY(0); }
    }
    @keyframes slideInRight {
      from { opacity: 0; transform: translateX(20px); }
      to { opacity: 1; transform: translateX(0); }
    }
    @keyframes pulseGlow {
      0%, 100% { box-shadow: 0 0 5px rgba(234,88,12,0.4); }
      50% { box-shadow: 0 0 20px rgba(234,88,12,0.6); }
    }
    @keyframes bounce {
      0%, 100% { transform: translateY(0) translateX(-50%); }
      50% { transform: translateY(-8px) translateX(-50%); }
    }
    .anim-fade { animation: fadeInUp 0.6s ease-out forwards; }
    .anim-slide { animation: slideInRight 0.4s ease-out forwards; }
    .anim-glow { animation: pulseGlow 2s ease-in-out infinite; }
    .anim-bounce { animation: bounce 2s infinite; }

    /* ===== NAVBAR ===== */
    .navbar {
      position: fixed; top: 0; left: 0; right: 0; z-index: 100;
      background: rgba(15,11,9,0.8); backdrop-filter: blur(12px);
      border-bottom: 1px solid var(--border);
    }
    .navbar-inner {
      max-width: 1280px; margin: 0 auto;
      display: flex; align-items: center; justify-content: space-between;
      padding: 0.75rem 1rem;
    }
    .navbar-brand { display: flex; align-items: center; gap: 0.5rem; }
    .navbar-logo {
      width: 40px; height: 40px; border-radius: 50%; background: var(--primary);
      display: flex; align-items: center; justify-content: center;
      color: var(--primary-fg); font-weight: 700; font-size: 1.1rem;
    }
    .navbar-name { font-weight: 700; font-size: 1.25rem; color: var(--fg); }
    .nav-links { display: flex; align-items: center; gap: 1.5rem; list-style: none; }
    .nav-links a {
      color: var(--muted-fg); font-size: 0.875rem; font-weight: 500;
      transition: color 0.2s;
    }
    .nav-links a:hover { color: var(--primary); }
    .cart-btn {
      position: relative; background: transparent; border: 1px solid var(--border);
      color: var(--fg); width: 40px; height: 40px; border-radius: var(--radius);
      display: flex; align-items: center; justify-content: center; transition: all 0.2s;
    }
    .cart-btn:hover { border-color: var(--primary); color: var(--primary); }
    .cart-badge {
      position: absolute; top: -8px; right: -8px;
      background: var(--primary); color: var(--primary-fg);
      font-size: 0.7rem; font-weight: 700; width: 20px; height: 20px;
      border-radius: 50%; display: flex; align-items: center; justify-content: center;
    }
    .cart-badge.hidden { display: none; }
    .menu-btn {
      display: none; background: transparent; border: none; color: var(--fg);
      width: 40px; height: 40px; align-items: center; justify-content: center;
    }
    .nav-actions { display: flex; align-items: center; gap: 0.75rem; }
    .mobile-nav {
      display: none; background: var(--card); border-top: 1px solid var(--border);
      padding: 1rem; list-style: none;
    }
    .mobile-nav.open { display: block; }
    .mobile-nav li a {
      display: block; padding: 0.5rem 0; color: var(--muted-fg);
      font-size: 0.875rem; font-weight: 500; transition: color 0.2s;
    }
    .mobile-nav li a:hover { color: var(--primary); }

    @media (max-width: 768px) {
      .nav-links { display: none; }
      .menu-btn { display: flex; }
      .navbar-name { display: none; }
    }

    /* ===== HERO ===== */
    .hero {
      position: relative; min-height: 100vh; display: flex; align-items: center;
      justify-content: center; overflow: hidden;
    }
    .hero-bg {
      position: absolute; inset: 0; background-size: cover;
      background-position: center;
      background-image: url('https://images.unsplash.com/photo-1568901346375-23c9450c58cd?w=1400&q=80');
    }
    .hero-overlay {
      position: absolute; inset: 0;
      background: linear-gradient(to bottom, rgba(15,11,9,0.8), rgba(15,11,9,0.6), var(--bg));
    }
    .hero-content {
      position: relative; z-index: 2; text-align: center;
      max-width: 800px; padding: 5rem 1rem 2rem;
    }
    .hero-tag {
      color: var(--primary); font-weight: 600; font-size: 0.8rem;
      letter-spacing: 0.15em; text-transform: uppercase; margin-bottom: 1rem;
    }
    .hero-title {
      font-family: 'Space Grotesk', sans-serif;
      font-size: clamp(2.5rem, 7vw, 4.5rem); font-weight: 700;
      line-height: 1.1; margin-bottom: 1.5rem; text-wrap: balance;
    }
    .hero-desc {
      color: var(--muted-fg); font-size: 1.125rem; max-width: 560px;
      margin: 0 auto 2rem; line-height: 1.6;
    }
    .hero-btns { display: flex; gap: 1rem; justify-content: center; flex-wrap: wrap; }
    .btn {
      display: inline-flex; align-items: center; justify-content: center; gap: 0.5rem;
      font-weight: 600; font-size: 0.9rem; border-radius: var(--radius); border: none;
      padding: 0.85rem 2rem; transition: all 0.2s;
    }
    .btn-primary { background: var(--primary); color: var(--primary-fg); }
    .btn-primary:hover { background: #c2410c; }
    .btn-outline {
      background: transparent; color: var(--fg);
      border: 1px solid var(--border);
    }
    .btn-outline:hover { border-color: var(--primary); color: var(--primary); }
    .btn-sm { padding: 0.5rem 1rem; font-size: 0.8rem; }
    .btn-full { width: 100%; padding: 1rem; font-size: 1rem; }
    .scroll-indicator {
      position: absolute; bottom: 2rem; left: 50%;
      transform: translateX(-50%); color: var(--muted-fg);
      transition: color 0.2s;
    }
    .scroll-indicator:hover { color: var(--primary); }

    /* ===== SECTIONS ===== */
    .section { padding: 5rem 1rem; }
    .section-alt { background: rgba(38,30,23,0.3); }
    .section-inner { max-width: 1280px; margin: 0 auto; }
    .section-header { text-align: center; margin-bottom: 4rem; }
    .section-tag {
      color: var(--primary); font-weight: 600; font-size: 0.8rem;
      letter-spacing: 0.15em; text-transform: uppercase; margin-bottom: 0.75rem;
    }
    .section-title {
      font-family: 'Space Grotesk', sans-serif;
      font-size: clamp(1.8rem, 5vw, 3rem); font-weight: 700; text-wrap: balance;
    }

    /* ===== MENU / MARKETPLACE ===== */
    .category-title {
      font-size: 1.5rem; font-weight: 700; margin-bottom: 2rem;
      display: flex; align-items: center; gap: 0.5rem;
    }
    .category-icon { color: var(--primary); }
    .products-grid {
      display: grid; gap: 1.5rem;
      grid-template-columns: repeat(auto-fill, minmax(260px, 1fr));
      margin-bottom: 4rem;
    }
    .product-card {
      background: var(--card); border: 1px solid var(--border);
      border-radius: var(--radius); overflow: hidden; transition: all 0.3s;
    }
    .product-card:hover { border-color: rgba(234,88,12,0.5); transform: translateY(-4px); }
    .product-img {
      width: 100%; height: 190px; object-fit: cover;
      transition: transform 0.5s;
    }
    .product-card:hover .product-img { transform: scale(1.1); }
    .product-img-wrap { position: relative; overflow: hidden; }
    .badge {
      position: absolute; top: 0.75rem; right: 0.75rem;
      background: var(--accent); color: var(--accent-fg);
      font-size: 0.7rem; font-weight: 600; padding: 0.25rem 0.6rem;
      border-radius: 9999px;
    }
    .badge-left { right: auto; left: 0.75rem; }
    .product-body { padding: 1.25rem; }
    .product-name { font-weight: 700; font-size: 1.05rem; margin-bottom: 0.25rem; }
    .product-desc {
      color: var(--muted-fg); font-size: 0.85rem; margin-bottom: 1rem;
      line-height: 1.5; display: -webkit-box; -webkit-line-clamp: 2;
      -webkit-box-orient: vertical; overflow: hidden;
    }
    .product-footer { display: flex; align-items: center; justify-content: space-between; }
    .product-price { color: var(--primary); font-weight: 700; font-size: 1.2rem; }

    /* ===== PROMOTIONS ===== */
    .promo-banner {
      background: rgba(234,88,12,0.1); border: 1px solid rgba(234,88,12,0.3);
      border-radius: var(--radius); padding: 1.5rem 2rem; margin-bottom: 3rem;
      display: flex; align-items: center; justify-content: space-between; gap: 1rem;
      flex-wrap: wrap;
    }
    .promo-banner-content { display: flex; align-items: center; gap: 1rem; }
    .promo-banner-icon { color: var(--primary); width: 40px; height: 40px; flex-shrink: 0; }
    .promo-banner h3 { font-weight: 700; font-size: 1.2rem; }
    .promo-banner p { color: var(--muted-fg); font-size: 0.9rem; }
    .promo-banner p span { color: var(--primary); font-weight: 700; }
    .promo-badge-avail {
      background: var(--primary); color: var(--primary-fg);
      font-size: 0.8rem; font-weight: 600; padding: 0.5rem 1rem;
      border-radius: 9999px; white-space: nowrap;
    }
    .combos-grid {
      display: grid; gap: 2rem;
      grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
    }
    .combo-items { list-style: none; margin-bottom: 1rem; }
    .combo-items li {
      display: flex; align-items: center; gap: 0.5rem;
      color: var(--secondary-fg); font-size: 0.85rem; padding: 0.15rem 0;
    }
    .combo-dot {
      width: 6px; height: 6px; border-radius: 50%;
      background: var(--primary); flex-shrink: 0;
    }
    .price-old {
      color: var(--muted-fg); text-decoration: line-through;
      font-size: 0.85rem; margin-right: 0.5rem;
    }

    /* ===== SURVEY ===== */
    .survey-inner { max-width: 700px; margin: 0 auto; }
    .stats-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 1rem; margin-bottom: 2.5rem; }
    .stat-card {
      background: var(--card); border: 1px solid var(--border);
      border-radius: var(--radius); padding: 1.5rem; text-align: center;
    }
    .stat-num { font-size: 1.8rem; font-weight: 700; color: var(--primary); }
    .stat-label { color: var(--muted-fg); font-size: 0.85rem; margin-top: 0.25rem; }
    .stat-num-fg { color: var(--fg); }
    .card {
      background: var(--card); border: 1px solid var(--border);
      border-radius: var(--radius); padding: 1.5rem;
    }
    .card-lg { padding: 2rem; }
    .form-label { font-weight: 600; display: block; margin-bottom: 0.75rem; }
    .stars { display: flex; gap: 0.5rem; }
    .star-btn {
      background: none; border: none; padding: 0;
      transition: transform 0.15s; color: var(--muted-fg);
    }
    .star-btn:hover { transform: scale(1.1); }
    .star-btn.active { color: var(--primary); }
    .star-hint { color: var(--muted-fg); font-size: 0.85rem; margin-top: 0.5rem; }
    .form-input, .form-select, .form-textarea {
      width: 100%; background: var(--secondary); color: var(--secondary-fg);
      border: none; border-radius: var(--radius); padding: 0.75rem 1rem;
      font-size: 0.875rem; font-family: inherit; outline: none;
      transition: box-shadow 0.2s;
    }
    .form-input::placeholder, .form-textarea::placeholder { color: var(--muted-fg); }
    .form-input:focus, .form-select:focus, .form-textarea:focus {
      box-shadow: 0 0 0 1px var(--primary);
    }
    .form-textarea { resize: none; }
    .form-group { margin-bottom: 2rem; }
    .success-msg {
      text-align: center; padding: 2.5rem 0;
    }
    .success-icon { color: var(--primary); margin-bottom: 1rem; }
    .success-msg h3 { font-size: 1.5rem; font-weight: 700; margin-bottom: 0.5rem; }
    .success-msg p { color: var(--muted-fg); }

    /* ===== SUGGESTIONS ===== */
    .suggestions-grid {
      display: grid; grid-template-columns: 1fr 1fr; gap: 2rem;
    }
    .sugg-icon-header { display: flex; align-items: center; gap: 0.75rem; margin-bottom: 1.5rem; }
    .sugg-icon-header svg { color: var(--primary); }
    .sugg-icon-header h3 { font-weight: 700; font-size: 1.1rem; }
    .type-toggle { display: flex; gap: 0.75rem; }
    .type-btn {
      flex: 1; padding: 0.65rem; border-radius: var(--radius); border: none;
      font-size: 0.85rem; font-weight: 500; transition: all 0.2s;
      background: var(--secondary); color: var(--secondary-fg);
    }
    .type-btn.active { background: var(--primary); color: var(--primary-fg); }
    .sugg-list { display: flex; flex-direction: column; gap: 0.75rem; max-height: 500px; overflow-y: auto; padding-right: 0.5rem; }
    .sugg-card {
      background: var(--card); border: 1px solid var(--border);
      border-radius: var(--radius); padding: 1rem; transition: border-color 0.2s;
    }
    .sugg-card:hover { border-color: rgba(234,88,12,0.3); }
    .sugg-card-header { display: flex; align-items: center; justify-content: space-between; margin-bottom: 0.5rem; }
    .sugg-card-name { font-weight: 500; font-size: 0.875rem; }
    .sugg-card-type {
      font-size: 0.7rem; padding: 0.15rem 0.5rem; border-radius: 9999px;
    }
    .sugg-card-type.product { background: rgba(220,38,38,0.2); color: var(--accent); }
    .sugg-card-type.general { background: rgba(234,88,12,0.2); color: var(--primary); }
    .sugg-card p { color: var(--muted-fg); font-size: 0.85rem; line-height: 1.5; }
    .sugg-card-date { color: rgba(136,122,109,0.6); font-size: 0.75rem; margin-top: 0.5rem; }
    .empty-sugg { text-align: center; padding: 2rem; color: var(--muted-fg); font-size: 0.875rem; }
    .alert-success {
      background: rgba(234,88,12,0.1); border: 1px solid rgba(234,88,12,0.3);
      color: var(--primary); border-radius: var(--radius); padding: 0.75rem;
      font-size: 0.85rem; margin-bottom: 1rem;
    }

    @media (max-width: 768px) {
      .suggestions-grid { grid-template-columns: 1fr; }
    }

    /* ===== FOOTER ===== */
    .footer { border-top: 1px solid var(--border); background: var(--card); }
    .footer-social { max-width: 1280px; margin: 0 auto; padding: 3rem 1rem; text-align: center; }
    .social-links { display: flex; align-items: center; justify-content: center; gap: 1.5rem; margin-top: 2rem; }
    .social-link {
      display: flex; align-items: center; gap: 0.75rem;
      background: var(--secondary); color: var(--secondary-fg);
      padding: 1rem 1.5rem; border-radius: var(--radius);
      transition: all 0.3s; border: none; font-weight: 500; font-size: 0.875rem;
    }
    .social-link:hover { transform: scale(1.05); color: var(--fg); }
    .social-link.fb:hover { background: #1877F2; }
    .social-link.ig:hover { background: #E4405F; }
    .social-link.wa:hover { background: #25D366; }
    .social-link svg { width: 24px; height: 24px; fill: currentColor; }
    .social-link span { display: none; }
    @media (min-width: 640px) { .social-link span { display: inline; } }
    .footer-bottom {
      border-top: 1px solid var(--border); max-width: 1280px; margin: 0 auto;
      padding: 1.5rem 1rem;
      display: flex; align-items: center; justify-content: space-between;
      flex-wrap: wrap; gap: 1rem;
    }
    .footer-brand { display: flex; align-items: center; gap: 0.5rem; }
    .footer-logo {
      width: 32px; height: 32px; border-radius: 50%; background: var(--primary);
      display: flex; align-items: center; justify-content: center;
      color: var(--primary-fg); font-weight: 700; font-size: 0.8rem;
    }
    .footer-brand span { font-weight: 700; }
    .footer-copy { color: var(--muted-fg); font-size: 0.8rem; }
    .footer-links { display: flex; gap: 1rem; }
    .footer-links a { color: var(--muted-fg); font-size: 0.8rem; transition: color 0.2s; }
    .footer-links a:hover { color: var(--primary); }

    /* ===== CHAT ===== */
    .chat-fab {
      position: fixed; bottom: 1.5rem; right: 1.5rem; z-index: 90;
      width: 56px; height: 56px; border-radius: 50%; border: none;
      background: var(--primary); color: var(--primary-fg);
      display: flex; align-items: center; justify-content: center;
      box-shadow: 0 4px 20px rgba(0,0,0,0.3);
    }
    .chat-panel {
      position: fixed; bottom: 1.5rem; right: 1.5rem; z-index: 90;
      width: 360px; max-width: calc(100vw - 2rem); max-height: calc(100vh - 3rem);
      background: var(--card); border: 1px solid var(--border);
      border-radius: var(--radius); box-shadow: 0 8px 40px rgba(0,0,0,0.4);
      display: flex; flex-direction: column; overflow: hidden;
    }
    .chat-panel.hidden { display: none; }
    .chat-header {
      display: flex; align-items: center; justify-content: space-between;
      padding: 1rem; border-bottom: 1px solid var(--border);
      background: rgba(38,30,23,0.5);
    }
    .chat-header-info { display: flex; align-items: center; gap: 0.75rem; }
    .chat-avatar {
      width: 36px; height: 36px; border-radius: 50%; background: var(--primary);
      display: flex; align-items: center; justify-content: center;
      color: var(--primary-fg);
    }
    .chat-header-text p:first-child { font-weight: 600; font-size: 0.875rem; }
    .chat-header-text p:last-child { color: var(--muted-fg); font-size: 0.75rem; }
    .chat-close {
      background: none; border: none; color: var(--muted-fg);
      width: 32px; height: 32px; display: flex; align-items: center; justify-content: center;
      border-radius: var(--radius); transition: all 0.2s;
    }
    .chat-close:hover { color: var(--fg); background: var(--secondary); }
    .chat-messages {
      flex: 1; overflow-y: auto; padding: 1rem;
      display: flex; flex-direction: column; gap: 0.75rem;
      min-height: 250px; max-height: 350px;
    }
    .chat-msg { display: flex; gap: 0.5rem; }
    .chat-msg.user { justify-content: flex-end; }
    .chat-msg.bot { justify-content: flex-start; }
    .msg-avatar {
      width: 28px; height: 28px; border-radius: 50%;
      display: flex; align-items: center; justify-content: center;
      flex-shrink: 0; margin-top: 0.25rem;
    }
    .msg-avatar.bot-av { background: rgba(234,88,12,0.2); color: var(--primary); }
    .msg-avatar.user-av { background: var(--muted); color: var(--muted-fg); }
    .msg-bubble {
      max-width: 75%; padding: 0.5rem 0.75rem; border-radius: var(--radius);
      font-size: 0.85rem; line-height: 1.5;
    }
    .msg-bubble.bot-b {
      background: var(--secondary); color: var(--secondary-fg);
      border-bottom-left-radius: 4px;
    }
    .msg-bubble.user-b {
      background: var(--primary); color: var(--primary-fg);
      border-bottom-right-radius: 4px;
    }
    .chat-quick { padding: 0 1rem 0.5rem; display: flex; flex-wrap: wrap; gap: 0.5rem; }
    .quick-btn {
      font-size: 0.75rem; background: var(--secondary); color: var(--secondary-fg);
      padding: 0.4rem 0.75rem; border-radius: 9999px; border: none;
      transition: all 0.2s;
    }
    .quick-btn:hover { background: rgba(234,88,12,0.2); color: var(--primary); }
    .chat-input-area {
      padding: 0.75rem; border-top: 1px solid var(--border);
      display: flex; align-items: center; gap: 0.5rem;
    }
    .chat-input {
      flex: 1; background: var(--secondary); color: var(--secondary-fg);
      border: none; border-radius: var(--radius); padding: 0.5rem 0.75rem;
      font-size: 0.85rem; font-family: inherit; outline: none;
    }
    .chat-input::placeholder { color: var(--muted-fg); }
    .chat-input:focus { box-shadow: 0 0 0 1px var(--primary); }
    .chat-send {
      width: 36px; height: 36px; flex-shrink: 0; border-radius: var(--radius);
      border: none; background: var(--primary); color: var(--primary-fg);
      display: flex; align-items: center; justify-content: center; transition: background 0.2s;
    }
    .chat-send:hover { background: #c2410c; }
    .chat-send:disabled { opacity: 0.5; cursor: not-allowed; }

    /* ===== CART DRAWER ===== */
    .cart-overlay {
      position: fixed; inset: 0; background: rgba(15,11,9,0.6);
      backdrop-filter: blur(4px); z-index: 110;
    }
    .cart-overlay.hidden { display: none; }
    .cart-drawer {
      position: fixed; right: 0; top: 0; bottom: 0; width: 100%; max-width: 420px;
      background: var(--card); border-left: 1px solid var(--border); z-index: 110;
      display: flex; flex-direction: column;
    }
    .cart-drawer.hidden { display: none; }
    .cart-header {
      display: flex; align-items: center; justify-content: space-between;
      padding: 1rem; border-bottom: 1px solid var(--border);
    }
    .cart-header-left { display: flex; align-items: center; gap: 0.5rem; }
    .cart-header-left svg { color: var(--primary); }
    .cart-header-left h2 { font-weight: 700; font-size: 1.1rem; }
    .cart-header-left span { color: var(--muted-fg); font-size: 0.85rem; }
    .cart-close {
      background: none; border: none; color: var(--muted-fg); width: 36px; height: 36px;
      display: flex; align-items: center; justify-content: center;
      border-radius: var(--radius); transition: all 0.2s;
    }
    .cart-close:hover { color: var(--fg); background: var(--secondary); }
    .cart-items { flex: 1; overflow-y: auto; padding: 1rem; }
    .cart-empty { text-align: center; padding: 4rem 1rem; color: var(--muted-fg); }
    .cart-empty svg { opacity: 0.3; margin: 0 auto 1rem; }
    .cart-item {
      display: flex; align-items: center; gap: 1rem;
      background: var(--secondary); border-radius: var(--radius); padding: 0.75rem;
      margin-bottom: 0.75rem;
    }
    .cart-item-info { flex: 1; min-width: 0; }
    .cart-item-name { font-weight: 500; font-size: 0.875rem; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
    .cart-item-price { color: var(--primary); font-weight: 700; font-size: 0.875rem; margin-top: 0.25rem; }
    .qty-controls { display: flex; align-items: center; gap: 0.5rem; }
    .qty-btn {
      width: 28px; height: 28px; border-radius: var(--radius); border: none;
      background: var(--muted); color: var(--muted-fg);
      display: flex; align-items: center; justify-content: center; transition: all 0.2s;
    }
    .qty-btn:hover { background: var(--primary); color: var(--primary-fg); }
    .qty-num { font-weight: 500; font-size: 0.875rem; width: 24px; text-align: center; }
    .remove-btn {
      background: none; border: none; color: var(--muted-fg); transition: color 0.2s;
      display: flex; align-items: center; justify-content: center;
    }
    .remove-btn:hover { color: var(--accent); }
    .cart-footer { border-top: 1px solid var(--border); padding: 1rem; }
    .cart-total {
      display: flex; align-items: center; justify-content: space-between;
      margin-bottom: 1rem;
    }
    .cart-total span:first-child { color: var(--muted-fg); font-weight: 500; }
    .cart-total span:last-child { font-weight: 700; font-size: 1.5rem; }

    /* ===== SVG ICONS INLINE ===== */
    .icon { width: 20px; height: 20px; display: inline-block; vertical-align: middle; }
    .icon-lg { width: 24px; height: 24px; }
    .icon-xl { width: 40px; height: 40px; }
    .icon-xxl { width: 64px; height: 64px; }

  </style>
</head>
<body>

  <!-- ===== NAVBAR ===== -->
  <header class="navbar">
    <nav class="navbar-inner">
      <a href="#inicio" class="navbar-brand">
        <div class="navbar-logo">SF</div>
        <span class="navbar-name">Samuel Food</span>
      </a>
      <ul class="nav-links">
        <li><a href="#inicio">Inicio</a></li>
        <li><a href="#menu">Menu</a></li>
        <li><a href="#promos">Promociones</a></li>
        <li><a href="#encuesta">Encuesta</a></li>
        <li><a href="#sugerencias">Sugerencias</a></li>
      </ul>
      <div class="nav-actions">
        <button class="cart-btn" onclick="toggleCart()" aria-label="Ver carrito">
          <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="8" cy="21" r="1"/><circle cx="19" cy="21" r="1"/><path d="M2.05 2.05h2l2.66 12.42a2 2 0 0 0 2 1.58h9.78a2 2 0 0 0 1.95-1.57l1.65-7.43H5.12"/></svg>
          <span class="cart-badge hidden" id="cartBadge">0</span>
        </button>
        <button class="menu-btn" onclick="toggleMobileNav()" aria-label="Menu de navegacion">
          <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><line x1="4" x2="20" y1="12" y2="12"/><line x1="4" x2="20" y1="6" y2="6"/><line x1="4" x2="20" y1="18" y2="18"/></svg>
        </button>
      </div>
    </nav>
    <ul class="mobile-nav" id="mobileNav">
      <li><a href="#inicio" onclick="closeMobileNav()">Inicio</a></li>
      <li><a href="#menu" onclick="closeMobileNav()">Menu</a></li>
      <li><a href="#promos" onclick="closeMobileNav()">Promociones</a></li>
      <li><a href="#encuesta" onclick="closeMobileNav()">Encuesta</a></li>
      <li><a href="#sugerencias" onclick="closeMobileNav()">Sugerencias</a></li>
    </ul>
  </header>

  <!-- ===== HERO ===== -->
  <section class="hero" id="inicio">
    <div class="hero-bg"></div>
    <div class="hero-overlay"></div>
    <div class="hero-content anim-fade">
      <p class="hero-tag">El mejor sabor de la ciudad</p>
      <h1 class="hero-title">Hamburguesas, Perros y Mucho Mas</h1>
      <p class="hero-desc">Disfruta de la mejor comida rapida con ingredientes frescos y un sabor que no olvidaras. Haz tu pedido ahora.</p>
      <div class="hero-btns">
        <a href="#menu" class="btn btn-primary anim-glow">Ver Menu</a>
        <a href="#promos" class="btn btn-outline">Promociones</a>
      </div>
    </div>
    <a href="#menu" class="scroll-indicator anim-bounce" aria-label="Desplazar hacia abajo">
      <svg xmlns="http://www.w3.org/2000/svg" width="32" height="32" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="m6 9 6 6 6-6"/></svg>
    </a>
  </section>

  <!-- ===== MENU ===== -->
  <section class="section" id="menu">
    <div class="section-inner">
      <div class="section-header">
        <p class="section-tag">Nuestro Menu</p>
        <h2 class="section-title">Elige tu favorito</h2>
      </div>
      <div id="menuContainer"></div>
    </div>
  </section>

  <!-- ===== PROMOCIONES ===== -->
  <section class="section section-alt" id="promos">
    <div class="section-inner">
      <div class="section-header">
        <p class="section-tag" style="color:var(--accent)">Ofertas Especiales</p>
        <h2 class="section-title">Combos y Promociones</h2>
      </div>
      <div class="promo-banner">
        <div class="promo-banner-content">
          <svg class="promo-banner-icon" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"/><polyline points="12 6 12 12 16 14"/></svg>
          <div>
            <h3>Promo del dia</h3>
            <p>Hamburguesa Doble + Salchipapa + Bebida por solo <span>$35.000</span></p>
          </div>
        </div>
        <span class="promo-badge-avail">Disponible hoy</span>
      </div>
      <div id="combosContainer" class="combos-grid"></div>
    </div>
  </section>

  <!-- ===== ENCUESTA ===== -->
  <section class="section" id="encuesta">
    <div class="section-inner">
      <div class="section-header">
        <p class="section-tag">Tu Opinion Importa</p>
        <h2 class="section-title">Encuesta de Satisfaccion</h2>
      </div>
      <div class="survey-inner">
        <div class="stats-grid" id="surveyStats" style="display:none">
          <div class="stat-card">
            <p class="stat-num" id="statAvg">0</p>
            <p class="stat-label">Calificacion promedio</p>
          </div>
          <div class="stat-card">
            <p class="stat-num stat-num-fg" id="statCount">0</p>
            <p class="stat-label">Encuestas recibidas</p>
          </div>
        </div>
        <div class="card card-lg" id="surveyCard">
          <div id="surveyForm">
            <div class="form-group">
              <label class="form-label">Que tan satisfecho estas con tu experiencia?</label>
              <div class="stars" id="starsContainer"></div>
              <p class="star-hint" id="starHint"></p>
            </div>
            <div class="form-group">
              <label class="form-label">Que producto te gusto mas?</label>
              <select class="form-select" id="favProduct">
                <option value="">Selecciona un producto</option>
              </select>
            </div>
            <div class="form-group">
              <label class="form-label">Comentario adicional</label>
              <textarea class="form-textarea" id="surveyComment" rows="3" placeholder="Cuentanos tu experiencia..."></textarea>
            </div>
            <button class="btn btn-primary btn-full" id="surveySubmit" disabled onclick="submitSurvey()">Enviar Encuesta</button>
          </div>
          <div class="success-msg" id="surveySuccess" style="display:none">
            <svg class="success-icon icon-xxl" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" style="margin:0 auto"><path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"/><polyline points="22 4 12 14.01 9 11.01"/></svg>
            <h3>Gracias por tu opinion!</h3>
            <p>Tu feedback nos ayuda a mejorar cada dia.</p>
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- ===== SUGERENCIAS ===== -->
  <section class="section section-alt" id="sugerencias">
    <div class="section-inner">
      <div class="section-header">
        <p class="section-tag">Queremos Escucharte</p>
        <h2 class="section-title">Sugerencias</h2>
      </div>
      <div class="suggestions-grid" style="max-width:900px;margin:0 auto">
        <div class="card card-lg">
          <div class="sugg-icon-header">
            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 14c.2-1 .7-1.7 1.5-2.5 1-.9 1.5-2.2 1.5-3.5A6 6 0 0 0 6 8c0 1 .2 2.2 1.5 3.5.7.7 1.3 1.5 1.5 2.5"/><path d="M9 18h6"/><path d="M10 22h4"/></svg>
            <h3>Envia tu sugerencia</h3>
          </div>
          <div id="suggAlert" class="alert-success" style="display:none">Gracias por tu sugerencia! La revisaremos pronto.</div>
          <div style="display:flex;flex-direction:column;gap:1rem">
            <input class="form-input" type="text" id="suggName" placeholder="Tu nombre (opcional)" />
            <div class="type-toggle">
              <button class="type-btn active" id="typeSugg" onclick="setSuggType('sugerencia')">Sugerencia</button>
              <button class="type-btn" id="typeProd" onclick="setSuggType('producto')">Nuevo producto</button>
            </div>
            <textarea class="form-textarea" id="suggText" rows="4" placeholder="Escribe tu sugerencia aqui..."></textarea>
            <button class="btn btn-primary btn-full" id="suggSubmit" disabled onclick="submitSuggestion()">
              <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" style="margin-right:0.5rem"><line x1="22" x2="11" y1="2" y2="13"/><polygon points="22 2 15 22 11 13 2 9 22 2"/></svg>
              Enviar
            </button>
          </div>
        </div>
        <div>
          <div class="sugg-icon-header">
            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M21 15a2 2 0 0 1-2 2H7l-4 4V5a2 2 0 0 1 2-2h14a2 2 0 0 1 2 2z"/></svg>
            <h3>Sugerencias recientes</h3>
          </div>
          <div class="sugg-list" id="suggList"></div>
        </div>
      </div>
    </div>
  </section>

  <!-- ===== FOOTER ===== -->
  <footer class="footer">
    <div class="footer-social">
      <p class="section-tag">Siguenos</p>
      <h2 class="section-title" style="font-size:clamp(1.5rem,3vw,2rem)">Conecta con nosotros</h2>
      <div class="social-links">
        <a href="https://www.facebook.com/share/1YuyNiwkjf/" target="_blank" rel="noopener noreferrer" class="social-link fb" aria-label="Visitanos en Facebook">
          <svg viewBox="0 0 24 24"><path d="M24 12.073c0-6.627-5.373-12-12-12s-12 5.373-12 12c0 5.99 4.388 10.954 10.125 11.854v-8.385H7.078v-3.47h3.047V9.43c0-3.007 1.792-4.669 4.533-4.669 1.312 0 2.686.235 2.686.235v2.953H15.83c-1.491 0-1.956.925-1.956 1.874v2.25h3.328l-.532 3.47h-2.796v8.385C19.612 23.027 24 18.062 24 12.073z"/></svg>
          <span>Facebook</span>
        </a>
        <a href="https://www.instagram.com/samuelflorez271?igsh=MWhuc2d3MDB3czFydg==" target="_blank" rel="noopener noreferrer" class="social-link ig" aria-label="Visitanos en Instagram">
          <svg viewBox="0 0 24 24"><path d="M12 2.163c3.204 0 3.584.012 4.85.07 3.252.148 4.771 1.691 4.919 4.919.058 1.265.069 1.645.069 4.849 0 3.205-.012 3.584-.069 4.849-.149 3.225-1.664 4.771-4.919 4.919-1.266.058-1.644.07-4.85.07-3.204 0-3.584-.012-4.849-.07-3.26-.149-4.771-1.699-4.919-4.92-.058-1.265-.07-1.644-.07-4.849 0-3.204.013-3.583.07-4.849.149-3.227 1.664-4.771 4.919-4.919 1.266-.057 1.645-.069 4.849-.069zM12 0C8.741 0 8.333.014 7.053.072 2.695.272.273 2.69.073 7.052.014 8.333 0 8.741 0 12c0 3.259.014 3.668.072 4.948.2 4.358 2.618 6.78 6.98 6.98C8.333 23.986 8.741 24 12 24c3.259 0 3.668-.014 4.948-.072 4.354-.2 6.782-2.618 6.979-6.98.059-1.28.073-1.689.073-4.948 0-3.259-.014-3.667-.072-4.947-.196-4.354-2.617-6.78-6.979-6.98C15.668.014 15.259 0 12 0zm0 5.838a6.162 6.162 0 100 12.324 6.162 6.162 0 000-12.324zM12 16a4 4 0 110-8 4 4 0 010 8zm6.406-11.845a1.44 1.44 0 100 2.881 1.44 1.44 0 000-2.881z"/></svg>
          <span>Instagram</span>
        </a>
        <a href="https://wa.me/qr/JXE3IVQYTTFVE1" target="_blank" rel="noopener noreferrer" class="social-link wa" aria-label="Visitanos en WhatsApp">
          <svg viewBox="0 0 24 24"><path d="M17.472 14.382c-.297-.149-1.758-.867-2.03-.967-.273-.099-.471-.148-.67.15-.197.297-.767.966-.94 1.164-.173.199-.347.223-.644.075-.297-.15-1.255-.463-2.39-1.475-.883-.788-1.48-1.761-1.653-2.059-.173-.297-.018-.458.13-.606.134-.133.298-.347.446-.52.149-.174.198-.298.298-.497.099-.198.05-.371-.025-.52-.075-.149-.669-1.612-.916-2.207-.242-.579-.487-.5-.669-.51-.173-.008-.371-.01-.57-.01-.198 0-.52.074-.792.372-.272.297-1.04 1.016-1.04 2.479 0 1.462 1.065 2.875 1.213 3.074.149.198 2.096 3.2 5.077 4.487.709.306 1.262.489 1.694.625.712.227 1.36.195 1.871.118.571-.085 1.758-.719 2.006-1.413.248-.694.248-1.289.173-1.413-.074-.124-.272-.198-.57-.347m-5.421 7.403h-.004a9.87 9.87 0 01-5.031-1.378l-.361-.214-3.741.982.998-3.648-.235-.374a9.86 9.86 0 01-1.51-5.26c.001-5.45 4.436-9.884 9.888-9.884 2.64 0 5.122 1.03 6.988 2.898a9.825 9.825 0 012.893 6.994c-.003 5.45-4.437 9.884-9.885 9.884m8.413-18.297A11.815 11.815 0 0012.05 0C5.495 0 .16 5.335.157 11.892c0 2.096.547 4.142 1.588 5.945L.057 24l6.305-1.654a11.882 11.882 0 005.683 1.448h.005c6.554 0 11.89-5.335 11.893-11.893a11.821 11.821 0 00-3.48-8.413z"/></svg>
          <span>WhatsApp</span>
        </a>
      </div>
    </div>
    <div class="footer-bottom">
      <div class="footer-brand">
        <div class="footer-logo">SF</div>
        <span>Samuel Food</span>
      </div>
      <p class="footer-copy">2024 Samuel Food. Todos los derechos reservados.</p>
      <div class="footer-links">
        <a href="#menu">Menu</a>
        <a href="#promos">Promos</a>
        <a href="#encuesta">Encuesta</a>
      </div>
    </div>
  </footer>

  <!-- ===== CHAT FAB ===== -->
  <button class="chat-fab anim-glow" id="chatFab" onclick="openChat()" aria-label="Abrir chat de asistencia">
    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="m3 21 1.9-5.7a8.5 8.5 0 1 1 3.8 3.8z"/></svg>
  </button>

  <!-- ===== CHAT PANEL ===== -->
  <div class="chat-panel hidden" id="chatPanel">
    <div class="chat-header">
      <div class="chat-header-info">
        <div class="chat-avatar">
          <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M12 8V4H8"/><rect width="16" height="12" x="4" y="8" rx="2"/><path d="M2 14h2"/><path d="M20 14h2"/><path d="M15 13v2"/><path d="M9 13v2"/></svg>
        </div>
        <div class="chat-header-text">
          <p>Asistente Samuel Food</p>
          <p>En linea</p>
        </div>
      </div>
      <button class="chat-close" onclick="closeChat()" aria-label="Cerrar chat">
        <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M18 6 6 18"/><path d="m6 6 12 12"/></svg>
      </button>
    </div>
    <div class="chat-messages" id="chatMessages">
      <div class="chat-msg bot">
        <div class="msg-avatar bot-av">
          <svg xmlns="http://www.w3.org/2000/svg" width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M12 8V4H8"/><rect width="16" height="12" x="4" y="8" rx="2"/><path d="M2 14h2"/><path d="M20 14h2"/><path d="M15 13v2"/><path d="M9 13v2"/></svg>
        </div>
        <div class="msg-bubble bot-b">Hola! Soy el asistente virtual de Samuel Food. Puedo recomendarte productos, contarte sobre promociones o resolver tus dudas. En que te puedo ayudar?</div>
      </div>
    </div>
    <div class="chat-quick">
      <button class="quick-btn" onclick="sendQuick('Recomiendame algo')">Recomiendame algo</button>
      <button class="quick-btn" onclick="sendQuick('Combos')">Combos</button>
      <button class="quick-btn" onclick="sendQuick('Precios')">Precios</button>
      <button class="quick-btn" onclick="sendQuick('Horario')">Horario</button>
    </div>
    <div class="chat-input-area">
      <input class="chat-input" type="text" id="chatInput" placeholder="Escribe tu pregunta..." onkeydown="if(event.key==='Enter')sendChatMsg()" />
      <button class="chat-send" id="chatSendBtn" onclick="sendChatMsg()" disabled aria-label="Enviar mensaje">
        <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><line x1="22" x2="11" y1="2" y2="13"/><polygon points="22 2 15 22 11 13 2 9 22 2"/></svg>
      </button>
    </div>
  </div>

  <!-- ===== CART OVERLAY ===== -->
  <div class="cart-overlay hidden" id="cartOverlay" onclick="closeCart()"></div>
  <div class="cart-drawer hidden anim-slide" id="cartDrawer">
    <div class="cart-header">
      <div class="cart-header-left">
        <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M6 2 3 6v14a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2V6l-3-4Z"/><path d="M3 6h18"/><path d="M16 10a4 4 0 0 1-8 0"/></svg>
        <h2>Tu Pedido</h2>
        <span id="cartCount">(0 items)</span>
      </div>
      <button class="cart-close" onclick="closeCart()" aria-label="Cerrar carrito">
        <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M18 6 6 18"/><path d="m6 6 12 12"/></svg>
      </button>
    </div>
    <div class="cart-items" id="cartItems"></div>
    <div class="cart-footer" id="cartFooter" style="display:none">
      <div class="cart-total">
        <span>Total</span>
        <span id="cartTotal">$0</span>
      </div>
      <button class="btn btn-primary btn-full" onclick="checkout()">Realizar Pedido por WhatsApp</button>
    </div>
  </div>

<script>
// ===== DATA =====
const products = [
  { id:1, name:"Hamburguesa Clasica", desc:"Carne 150g, queso cheddar, lechuga, tomate y salsa especial", price:15000, img:"https://images.unsplash.com/photo-1568901346375-23c9450c58cd?w=400&q=80", cat:"Hamburguesas", popular:true },
  { id:2, name:"Hamburguesa Doble", desc:"Doble carne 300g, doble queso, cebolla caramelizada y bacon", price:22000, img:"https://images.unsplash.com/photo-1553979459-d2229ba7433b?w=400&q=80", cat:"Hamburguesas" },
  { id:3, name:"Perro Caliente Especial", desc:"Salchicha premium, papitas, queso, salsas y tocineta", price:12000, img:"https://comedera.com/wp-content/uploads/sites/9/2022/03/Pan-de-perro-caliente-shutterstock_1472131574.jpg", cat:"Perros", popular:true },
  { id:4, name:"Perro Sencillo", desc:"Salchicha, salsas rosada, ketchup y mostaza", price:8000, img:"https://comedera.com/wp-content/uploads/sites/9/2022/03/Pan-de-perro-caliente-shutterstock_1472131574.jpg", cat:"Perros" },
  { id:5, name:"Salchipapa Grande", desc:"Papas fritas crujientes, salchichas, queso y todas las salsas", price:14000, img:"https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRshFKImV3MLn65nekuBa5Iu4s8gUuIQBQi6g&s", cat:"Salchipapas", popular:true },
  { id:6, name:"Salchipapa Mixta", desc:"Papas, salchichas, carne desmechada, queso y salsas", price:18000, img:"https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRshFKImV3MLn65nekuBa5Iu4s8gUuIQBQi6g&s", cat:"Salchipapas" },
  { id:7, name:"Sandwich de Pollo", desc:"Pollo a la plancha, lechuga, tomate, queso y mayonesa", price:13000, img:"https://images.unsplash.com/photo-1528735602780-2552fd46c7af?w=400&q=80", cat:"Sandwiches" },
  { id:8, name:"Sandwich Club", desc:"Jamon, pollo, queso, bacon, lechuga y tomate en pan tostado", price:16000, img:"https://images.unsplash.com/photo-1539252554453-80ab65ce3586?w=400&q=80", cat:"Sandwiches", popular:true },
];

const combos = [
  { id:101, name:"Combo Clasico", desc:"La combinacion perfecta para cualquier momento", orig:28000, price:22000, img:"https://images.unsplash.com/photo-1594212699903-ec8a3eca50f5?w=400&q=80", items:["Hamburguesa Clasica","Papas fritas","Gaseosa 400ml"] },
  { id:102, name:"Combo Familiar", desc:"Para compartir en familia con todo el sabor", orig:55000, price:42000, img:"https://images.unsplash.com/photo-1594212699903-ec8a3eca50f5?w=400&q=80", items:["2 Hamburguesas Dobles","Salchipapa Grande","2 Gaseosas"] },
  { id:103, name:"Combo Perrero", desc:"Para los amantes de los perros calientes", orig:32000, price:25000, img:"https://comedera.com/wp-content/uploads/sites/9/2022/03/Pan-de-perro-caliente-shutterstock_1472131574.jpg", items:["2 Perros Especiales","Papas fritas","Gaseosa 400ml"] },
];

let cart = [];
let surveyRating = 0;
let suggType = "sugerencia";

// ===== HELPERS =====
function formatPrice(n) {
  return new Intl.NumberFormat("es-CO",{style:"currency",currency:"COP",minimumFractionDigits:0}).format(n);
}

// ===== MOBILE NAV =====
function toggleMobileNav() { document.getElementById("mobileNav").classList.toggle("open"); }
function closeMobileNav() { document.getElementById("mobileNav").classList.remove("open"); }

// ===== RENDER MENU =====
function renderMenu() {
  const container = document.getElementById("menuContainer");
  const cats = [...new Set(products.map(p=>p.cat))];
  let html = "";
  cats.forEach(cat => {
    html += `<h3 class="category-title"><svg class="category-icon" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M8.5 14.5A2.5 2.5 0 0 0 11 12c0-1.38-.5-2-1-3-1.072-2.143-.224-4.054 2-6 .5 2.5 2 4.9 4 6.5 2 1.6 3 3.5 3 5.5a7 7 0 1 1-14 0c0-1.153.433-2.294 1-3a2.5 2.5 0 0 0 2.5 2.5z"/></svg>${cat}</h3><div class="products-grid">`;
    products.filter(p=>p.cat===cat).forEach(p => {
      html += `
        <div class="product-card">
          <div class="product-img-wrap">
            <img class="product-img" src="${p.img}" alt="${p.name}" loading="lazy" />
            ${p.popular?'<span class="badge">Popular</span>':''}
          </div>
          <div class="product-body">
            <h4 class="product-name">${p.name}</h4>
            <p class="product-desc">${p.desc}</p>
            <div class="product-footer">
              <span class="product-price">${formatPrice(p.price)}</span>
              <button class="btn btn-primary btn-sm" onclick="addToCart(${p.id})">
                <svg xmlns="http://www.w3.org/2000/svg" width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M5 12h14"/><path d="M12 5v14"/></svg>
                Agregar
              </button>
            </div>
          </div>
        </div>`;
    });
    html += `</div>`;
  });
  container.innerHTML = html;
}

// ===== RENDER COMBOS =====
function renderCombos() {
  const container = document.getElementById("combosContainer");
  let html = "";
  combos.forEach(c => {
    const disc = Math.round(((c.orig - c.price) / c.orig) * 100);
    html += `
      <div class="product-card">
        <div class="product-img-wrap">
          <img class="product-img" src="${c.img}" alt="${c.name}" loading="lazy" />
          <span class="badge badge-left">-${disc}%</span>
        </div>
        <div class="product-body">
          <h4 class="product-name">${c.name}</h4>
          <p class="product-desc">${c.desc}</p>
          <ul class="combo-items">${c.items.map(i=>`<li><span class="combo-dot"></span>${i}</li>`).join('')}</ul>
          <div class="product-footer">
            <div><span class="price-old">${formatPrice(c.orig)}</span><span class="product-price">${formatPrice(c.price)}</span></div>
            <button class="btn btn-primary btn-sm" onclick="addComboToCart(${c.id})">Agregar</button>
          </div>
        </div>
      </div>`;
  });
  container.innerHTML = html;
}

// ===== CART =====
function addToCart(productId) {
  const p = products.find(x=>x.id===productId);
  if(!p) return;
  const existing = cart.find(x=>x.id===p.id);
  if(existing) { existing.qty++; } else { cart.push({...p, qty:1}); }
  updateCartUI();
}

function addComboToCart(comboId) {
  const c = combos.find(x=>x.id===comboId);
  if(!c) return;
  const existing = cart.find(x=>x.id===c.id);
  if(existing) { existing.qty++; } else { cart.push({id:c.id, name:c.name, desc:c.desc, price:c.price, img:c.img, cat:"Combos", qty:1}); }
  updateCartUI();
}

function updateCartUI() {
  const totalQty = cart.reduce((s,i)=>s+i.qty,0);
  const badge = document.getElementById("cartBadge");
  badge.textContent = totalQty;
  badge.classList.toggle("hidden", totalQty===0);
  document.getElementById("cartCount").textContent = `(${totalQty} items)`;

  const itemsEl = document.getElementById("cartItems");
  const footerEl = document.getElementById("cartFooter");

  if(cart.length===0) {
    itemsEl.innerHTML = `<div class="cart-empty"><svg xmlns="http://www.w3.org/2000/svg" width="64" height="64" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M6 2 3 6v14a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2V6l-3-4Z"/><path d="M3 6h18"/><path d="M16 10a4 4 0 0 1-8 0"/></svg><p>Tu carrito esta vacio</p><p style="font-size:0.75rem;margin-top:0.25rem">Agrega productos del menu para empezar</p></div>`;
    footerEl.style.display = "none";
    return;
  }

  let html = "";
  cart.forEach(item => {
    html += `
      <div class="cart-item">
        <div class="cart-item-info">
          <p class="cart-item-name">${item.name}</p>
          <p class="cart-item-price">${formatPrice(item.price)}</p>
        </div>
        <div class="qty-controls">
          <button class="qty-btn" onclick="changeQty(${item.id},-1)" aria-label="Reducir cantidad">
            <svg xmlns="http://www.w3.org/2000/svg" width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M5 12h14"/></svg>
          </button>
          <span class="qty-num">${item.qty}</span>
          <button class="qty-btn" onclick="changeQty(${item.id},1)" aria-label="Aumentar cantidad">
            <svg xmlns="http://www.w3.org/2000/svg" width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M5 12h14"/><path d="M12 5v14"/></svg>
          </button>
        </div>
        <button class="remove-btn" onclick="removeItem(${item.id})" aria-label="Eliminar producto">
          <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M3 6h18"/><path d="M19 6v14c0 1-1 2-2 2H7c-1 0-2-1-2-2V6"/><path d="M8 6V4c0-1 1-2 2-2h4c1 0 2 1 2 2v2"/></svg>
        </button>
      </div>`;
  });
  itemsEl.innerHTML = html;

  const total = cart.reduce((s,i)=>s+i.price*i.qty,0);
  document.getElementById("cartTotal").textContent = formatPrice(total);
  footerEl.style.display = "block";
}

function changeQty(id, delta) {
  const item = cart.find(x=>x.id===id);
  if(!item) return;
  item.qty += delta;
  if(item.qty <= 0) cart = cart.filter(x=>x.id!==id);
  updateCartUI();
}

function removeItem(id) { cart = cart.filter(x=>x.id!==id); updateCartUI(); }
function toggleCart() { const d=document.getElementById("cartDrawer"); const o=document.getElementById("cartOverlay"); d.classList.toggle("hidden"); o.classList.toggle("hidden"); updateCartUI(); }
function closeCart() { document.getElementById("cartDrawer").classList.add("hidden"); document.getElementById("cartOverlay").classList.add("hidden"); }

function checkout() {
  if(cart.length===0) return;
  try { localStorage.setItem("sf_order_history", JSON.stringify(cart.map(i=>i.name))); } catch(e){}
  const total = cart.reduce((s,i)=>s+i.price*i.qty,0);
  let msg = "Hola! Quiero hacer un pedido:\n\n";
  cart.forEach(i => { msg += `- ${i.name} x${i.qty} = ${formatPrice(i.price*i.qty)}\n`; });
  msg += `\nTotal: ${formatPrice(total)}`;
  window.open(`https://wa.me/qr/JXE3IVQYTTFVE1?text=${encodeURIComponent(msg)}`,"_blank");
  cart = [];
  updateCartUI();
  closeCart();
}

// ===== CHAT =====
function openChat() { document.getElementById("chatPanel").classList.remove("hidden"); document.getElementById("chatFab").style.display="none"; }
function closeChat() { document.getElementById("chatPanel").classList.add("hidden"); document.getElementById("chatFab").style.display="flex"; }

const chatInput = document.getElementById("chatInput");
chatInput.addEventListener("input",()=>{ document.getElementById("chatSendBtn").disabled = !chatInput.value.trim(); });

const faq = {
  horario:"Estamos abiertos de lunes a sabado de 11:00 AM a 10:00 PM y domingos de 12:00 PM a 9:00 PM.",
  domicilio:"Si, hacemos domicilios. El costo de envio depende de tu ubicacion. Escribenos por WhatsApp para mas info.",
  pago:"Aceptamos efectivo, Nequi, Daviplata y transferencias bancarias.",
  ubicacion:"Estamos ubicados en el centro de la ciudad. Escribenos por WhatsApp para la direccion exacta.",
  ingredientes:"Usamos ingredientes 100% frescos. Nuestras carnes son de primera calidad y las verduras del dia.",
};

function getAIResponse(input) {
  const l = input.toLowerCase();
  for(const [k,v] of Object.entries(faq)) { if(l.includes(k)) return v; }
  if(/hola|hey|buenas|saludos/.test(l)) return "Hola! Bienvenido a Samuel Food. Soy tu asistente virtual. Puedo ayudarte a elegir tu comida, recomendarte combos o responder tus preguntas. Que deseas?";
  if(/recomiend|mejor|popular|suger/.test(l)) { const pop=products.filter(p=>p.popular).map(p=>p.name).join(", "); return `Nuestros productos mas populares son: ${pop}. Todos son excelentes opciones!`; }
  if(/combo|promo|oferta|descuento/.test(l)) return "Tenemos combos increibles! El Combo Clasico (Hamburguesa + Papas + Gaseosa) por $22.000 y el Combo Familiar por $42.000. Revisa la seccion de Promociones.";
  if(/precio|cuanto|cuesta|vale/.test(l)) return "Los precios van desde $8.000 (Perro Sencillo) hasta $22.000 (Hamburguesa Doble). Revisa nuestro menu para ver todos los precios.";
  if(/hambur|burger/.test(l)) return "Tenemos la Hamburguesa Clasica ($15.000) con carne de 150g y la Hamburguesa Doble ($22.000) con 300g de carne y bacon. Cual te gustaria?";
  if(/perro|hotdog|hot dog/.test(l)) return "Nuestros perros calientes son deliciosos! El Perro Especial ($12.000) viene con papitas, queso y tocineta. El Perro Sencillo ($8.000) es perfecto si buscas algo rapido.";
  if(/salchi|papa/.test(l)) return "Las salchipapas son un clasico! La Grande ($14.000) y la Mixta ($18.000) con carne desmechada. Ambas vienen con todas las salsas!";
  if(/sandwich|sanduche/.test(l)) return "Tenemos el Sandwich de Pollo ($13.000) y el Sandwich Club ($16.000) con jamon, pollo, queso y bacon.";
  if(/gracias|thanks/.test(l)) return "De nada! Estoy aqui para ayudarte. Que mas necesitas?";
  try { const hist=JSON.parse(localStorage.getItem("sf_order_history")||"[]"); if(hist.length>0&&/otra vez|de nuevo|repetir/.test(l)) return `La ultima vez pediste: ${hist.join(", ")}. Quieres repetir ese pedido?`; } catch(e){}
  return "Puedo ayudarte con: recomendaciones de productos, informacion sobre combos y promociones, precios, horarios, formas de pago y domicilios. Que te gustaria saber?";
}

function addChatMsg(text, sender) {
  const el = document.getElementById("chatMessages");
  const botIcon = `<svg xmlns="http://www.w3.org/2000/svg" width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M12 8V4H8"/><rect width="16" height="12" x="4" y="8" rx="2"/><path d="M2 14h2"/><path d="M20 14h2"/><path d="M15 13v2"/><path d="M9 13v2"/></svg>`;
  const userIcon = `<svg xmlns="http://www.w3.org/2000/svg" width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M19 21v-2a4 4 0 0 0-4-4H9a4 4 0 0 0-4 4v2"/><circle cx="12" cy="7" r="4"/></svg>`;

  let html = `<div class="chat-msg ${sender}">`;
  if(sender==="bot") html += `<div class="msg-avatar bot-av">${botIcon}</div>`;
  html += `<div class="msg-bubble ${sender==="bot"?"bot-b":"user-b"}">${text}</div>`;
  if(sender==="user") html += `<div class="msg-avatar user-av">${userIcon}</div>`;
  html += `</div>`;
  el.insertAdjacentHTML("beforeend", html);
  el.scrollTop = el.scrollHeight;
}

function sendChatMsg() {
  const input = document.getElementById("chatInput");
  const text = input.value.trim();
  if(!text) return;
  addChatMsg(text, "user");
  input.value = "";
  document.getElementById("chatSendBtn").disabled = true;
  setTimeout(()=>{ addChatMsg(getAIResponse(text), "bot"); }, 600);
}

function sendQuick(text) { addChatMsg(text,"user"); setTimeout(()=>{ addChatMsg(getAIResponse(text),"bot"); },600); }

// ===== SURVEY =====
function renderStars() {
  const c = document.getElementById("starsContainer");
  let html = "";
  for(let i=1;i<=5;i++) {
    html += `<button class="star-btn" data-star="${i}" onmouseenter="hoverStar(${i})" onmouseleave="unhoverStar()" onclick="setStar(${i})" aria-label="${i} estrellas">
      <svg xmlns="http://www.w3.org/2000/svg" width="40" height="40" viewBox="0 0 24 24" fill="${i<=surveyRating?'currentColor':'none'}" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polygon points="12 2 15.09 8.26 22 9.27 17 14.14 18.18 21.02 12 17.77 5.82 21.02 7 14.14 2 9.27 8.91 8.26 12 2"/></svg>
    </button>`;
  }
  c.innerHTML = html;

  // fill product select
  const sel = document.getElementById("favProduct");
  sel.innerHTML = '<option value="">Selecciona un producto</option>';
  products.forEach(p => { sel.innerHTML += `<option value="${p.name}">${p.name}</option>`; });
}

function hoverStar(n) {
  document.querySelectorAll(".star-btn").forEach((btn,i) => {
    const svg = btn.querySelector("svg");
    svg.setAttribute("fill", i<n?"currentColor":"none");
    btn.classList.toggle("active", i<n);
  });
}
function unhoverStar() {
  document.querySelectorAll(".star-btn").forEach((btn,i) => {
    const svg = btn.querySelector("svg");
    svg.setAttribute("fill", i<surveyRating?"currentColor":"none");
    btn.classList.toggle("active", i<surveyRating);
  });
}
function setStar(n) {
  surveyRating = n;
  document.getElementById("surveySubmit").disabled = false;
  unhoverStar();
  const hints = ["","Lamentamos tu experiencia, queremos mejorar.","Lamentamos tu experiencia, queremos mejorar.","Gracias! Seguiremos mejorando.","Excelente! Nos alegra que te haya gustado.","Excelente! Nos alegra que te haya gustado."];
  document.getElementById("starHint").textContent = hints[n]||"";
}

function loadSurveyStats() {
  try {
    const surveys = JSON.parse(localStorage.getItem("sf_surveys")||"[]");
    if(surveys.length>0) {
      document.getElementById("surveyStats").style.display="grid";
      const avg = (surveys.reduce((a,b)=>a+b.satisfaction,0)/surveys.length).toFixed(1);
      document.getElementById("statAvg").textContent = avg;
      document.getElementById("statCount").textContent = surveys.length;
    }
  } catch(e){}
}

function submitSurvey() {
  if(surveyRating===0) return;
  const survey = {
    satisfaction: surveyRating,
    favoriteProduct: document.getElementById("favProduct").value,
    comment: document.getElementById("surveyComment").value,
    date: new Date().toLocaleDateString("es-CO")
  };
  try {
    const surveys = JSON.parse(localStorage.getItem("sf_surveys")||"[]");
    surveys.push(survey);
    localStorage.setItem("sf_surveys", JSON.stringify(surveys));
  } catch(e){}
  document.getElementById("surveyForm").style.display = "none";
  document.getElementById("surveySuccess").style.display = "block";
  loadSurveyStats();
  setTimeout(()=>{
    document.getElementById("surveyForm").style.display = "block";
    document.getElementById("surveySuccess").style.display = "none";
    surveyRating = 0;
    document.getElementById("favProduct").value = "";
    document.getElementById("surveyComment").value = "";
    document.getElementById("surveySubmit").disabled = true;
    renderStars();
  }, 4000);
}

// ===== SUGGESTIONS =====
function setSuggType(type) {
  suggType = type;
  document.getElementById("typeSugg").classList.toggle("active", type==="sugerencia");
  document.getElementById("typeProd").classList.toggle("active", type==="producto");
  document.getElementById("suggText").placeholder = type==="sugerencia" ? "Escribe tu sugerencia aqui..." : "Que producto nuevo te gustaria ver?";
}

document.getElementById("suggText").addEventListener("input", function() {
  document.getElementById("suggSubmit").disabled = !this.value.trim();
});

function loadSuggestions() {
  try {
    const suggs = JSON.parse(localStorage.getItem("sf_suggestions")||"[]");
    renderSuggestions(suggs);
  } catch(e){ renderSuggestions([]); }
}

function renderSuggestions(suggs) {
  const list = document.getElementById("suggList");
  if(suggs.length===0) {
    list.innerHTML = `<div class="card"><div class="empty-sugg">Aun no hay sugerencias. Se el primero en compartir tu idea!</div></div>`;
    return;
  }
  let html = "";
  suggs.slice(0,10).forEach(s => {
    html += `
      <div class="sugg-card">
        <div class="sugg-card-header">
          <span class="sugg-card-name">${s.name}</span>
          <span class="sugg-card-type ${s.type==='producto'?'product':'general'}">${s.type==="producto"?"Nuevo producto":"Sugerencia"}</span>
        </div>
        <p>${s.text}</p>
        <p class="sugg-card-date">${s.date}</p>
      </div>`;
  });
  list.innerHTML = html;
}

function submitSuggestion() {
  const text = document.getElementById("suggText").value.trim();
  if(!text) return;
  const suggestion = {
    id: Date.now(),
    name: document.getElementById("suggName").value.trim() || "Anonimo",
    type: suggType,
    text: text,
    date: new Date().toLocaleDateString("es-CO")
  };
  try {
    const suggs = JSON.parse(localStorage.getItem("sf_suggestions")||"[]");
    suggs.unshift(suggestion);
    localStorage.setItem("sf_suggestions", JSON.stringify(suggs));
    renderSuggestions(suggs);
  } catch(e){}
  document.getElementById("suggName").value = "";
  document.getElementById("suggText").value = "";
  document.getElementById("suggSubmit").disabled = true;
  const alert = document.getElementById("suggAlert");
  alert.style.display = "block";
  setTimeout(()=>{ alert.style.display="none"; }, 3000);
}

// ===== INIT =====
renderMenu();
renderCombos();
renderStars();
loadSurveyStats();
loadSuggestions();
updateCartUI();
</script>
</body>
</html>
