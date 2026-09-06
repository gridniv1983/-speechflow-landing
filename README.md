<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>SpeechFlow — CRM для частных логопедов в Notion</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&family=Playfair+Display:wght@600;700&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #0c0c0e;
    --bg-card: #141416;
    --bg-elevated: #1a1a1e;
    --text: #f5f5f7;
    --text-secondary: #8e8e93;
    --text-muted: #636366;
    --accent: #d4a574;
    --accent-light: #e8c9a0;
    --accent-glow: rgba(212,165,116,0.15);
    --border: rgba(255,255,255,0.06);
    --border-hover: rgba(255,255,255,0.12);
    --success: #34c759;
    --gradient-start: #d4a574;
    --gradient-end: #c49a6c;
  }
  * { margin: 0; padding: 0; box-sizing: border-box; }
  html { scroll-behavior: smooth; }
  body {
    font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
    background: var(--bg);
    color: var(--text);
    line-height: 1.6;
    overflow-x: hidden;
  }
  ::selection { background: var(--accent); color: #000; }

  /* Noise overlay */
  body::before {
    content: '';
    position: fixed;
    top: 0; left: 0; width: 100%; height: 100%;
    background: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)' opacity='0.03'/%3E%3C/svg%3E");
    pointer-events: none;
    z-index: 9999;
  }

  /* Navigation */
  nav {
    position: fixed;
    top: 0; left: 0; right: 0;
    z-index: 100;
    padding: 20px 40px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    background: rgba(12,12,14,0.8);
    backdrop-filter: blur(20px);
    border-bottom: 1px solid var(--border);
  }
  .logo {
    font-family: 'Playfair Display', serif;
    font-size: 22px;
    font-weight: 700;
    color: var(--text);
    letter-spacing: -0.5px;
  }
  .logo span { color: var(--accent); }
  .nav-links { display: flex; gap: 32px; align-items: center; }
  .nav-links a {
    color: var(--text-secondary);
    text-decoration: none;
    font-size: 14px;
    font-weight: 500;
    transition: color 0.3s;
  }
  .nav-links a:hover { color: var(--text); }
  .nav-cta {
    background: linear-gradient(135deg, var(--gradient-start), var(--gradient-end));
    color: #000 !important;
    padding: 10px 24px;
    border-radius: 8px;
    font-weight: 600;
    font-size: 13px;
    transition: transform 0.2s, box-shadow 0.2s;
  }
  .nav-cta:hover {
    transform: translateY(-1px);
    box-shadow: 0 8px 24px var(--accent-glow);
  }

  /* Hero */
  .hero {
    min-height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 140px 40px 80px;
    position: relative;
    overflow: hidden;
  }
  .hero-bg {
    position: absolute;
    top: -50%; left: -20%;
    width: 140%; height: 140%;
    background: radial-gradient(ellipse at 30% 20%, rgba(212,165,116,0.08) 0%, transparent 50%),
                radial-gradient(ellipse at 70% 80%, rgba(212,165,116,0.04) 0%, transparent 40%);
    pointer-events: none;
  }
  .hero-content {
    max-width: 720px;
    text-align: center;
    position: relative;
    z-index: 1;
  }
  .badge {
    display: inline-flex;
    align-items: center;
    gap: 8px;
    padding: 8px 18px;
    background: var(--bg-elevated);
    border: 1px solid var(--border);
    border-radius: 100px;
    font-size: 12px;
    font-weight: 500;
    color: var(--accent);
    margin-bottom: 32px;
    letter-spacing: 0.5px;
    text-transform: uppercase;
  }
  .badge::before {
    content: '';
    width: 6px; height: 6px;
    background: var(--accent);
    border-radius: 50%;
    animation: pulse 2s infinite;
  }
  @keyframes pulse {
    0%, 100% { opacity: 1; }
    50% { opacity: 0.4; }
  }
  h1 {
    font-family: 'Playfair Display', serif;
    font-size: clamp(36px, 5vw, 64px);
    font-weight: 700;
    line-height: 1.1;
    letter-spacing: -1.5px;
    margin-bottom: 24px;
  }
  h1 .highlight {
    background: linear-gradient(135deg, var(--gradient-start), var(--gradient-end));
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
  }
  .hero-subtitle {
    font-size: clamp(16px, 2vw, 20px);
    color: var(--text-secondary);
    font-weight: 400;
    line-height: 1.7;
    max-width: 560px;
    margin: 0 auto 40px;
  }
  .hero-cta-group {
    display: flex;
    gap: 16px;
    justify-content: center;
    flex-wrap: wrap;
    margin-bottom: 48px;
  }
  .btn-primary {
    background: linear-gradient(135deg, var(--gradient-start), var(--gradient-end));
    color: #000;
    padding: 16px 40px;
    border-radius: 12px;
    font-size: 15px;
    font-weight: 600;
    text-decoration: none;
    display: inline-flex;
    align-items: center;
    gap: 8px;
    transition: all 0.3s;
    border: none;
    cursor: pointer;
  }
  .btn-primary:hover {
    transform: translateY(-2px);
    box-shadow: 0 12px 40px var(--accent-glow);
  }
  .btn-secondary {
    background: transparent;
    color: var(--text);
    padding: 16px 40px;
    border-radius: 12px;
    font-size: 15px;
    font-weight: 500;
    text-decoration: none;
    display: inline-flex;
    align-items: center;
    gap: 8px;
    border: 1px solid var(--border);
    transition: all 0.3s;
  }
  .btn-secondary:hover {
    border-color: var(--border-hover);
    background: var(--bg-elevated);
  }
  .hero-stats {
    display: flex;
    gap: 48px;
    justify-content: center;
    flex-wrap: wrap;
  }
  .stat {
    text-align: center;
  }
  .stat-number {
    font-size: 32px;
    font-weight: 700;
    color: var(--accent);
    font-family: 'Playfair Display', serif;
  }
  .stat-label {
    font-size: 12px;
    color: var(--text-muted);
    text-transform: uppercase;
    letter-spacing: 1px;
    margin-top: 4px;
  }

  /* Sections */
  section { padding: 100px 40px; }
  .container { max-width: 1100px; margin: 0 auto; }
  .section-header {
    text-align: center;
    margin-bottom: 64px;
  }
  .section-label {
    font-size: 12px;
    font-weight: 600;
    color: var(--accent);
    text-transform: uppercase;
    letter-spacing: 2px;
    margin-bottom: 16px;
  }
  .section-title {
    font-family: 'Playfair Display', serif;
    font-size: clamp(28px, 3.5vw, 44px);
    font-weight: 700;
    line-height: 1.2;
    margin-bottom: 16px;
  }
  .section-desc {
    color: var(--text-secondary);
    font-size: 17px;
    max-width: 560px;
    margin: 0 auto;
  }

  /* Problem */
  .problem-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
    gap: 24px;
  }
  .problem-card {
    background: var(--bg-card);
    border: 1px solid var(--border);
    border-radius: 16px;
    padding: 32px;
    transition: all 0.3s;
  }
  .problem-card:hover {
    border-color: var(--border-hover);
    transform: translateY(-4px);
  }
  .problem-icon {
    width: 48px; height: 48px;
    background: rgba(255,59,48,0.1);
    border-radius: 12px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 24px;
    margin-bottom: 20px;
  }
  .problem-card h3 {
    font-size: 17px;
    font-weight: 600;
    margin-bottom: 10px;
    color: var(--text);
  }
  .problem-card p {
    font-size: 14px;
    color: var(--text-secondary);
    line-height: 1.6;
  }

  /* Solution */
  .solution-wrapper {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 64px;
    align-items: center;
  }
  @media (max-width: 900px) {
    .solution-wrapper { grid-template-columns: 1fr; }
  }
  .solution-visual {
    background: var(--bg-card);
    border: 1px solid var(--border);
    border-radius: 20px;
    padding: 32px;
    position: relative;
    overflow: hidden;
  }
  .solution-visual::before {
    content: '';
    position: absolute;
    top: -50%; left: -50%;
    width: 200%; height: 200%;
    background: radial-gradient(circle, var(--accent-glow) 0%, transparent 60%);
    pointer-events: none;
  }
  .notion-mock {
    background: var(--bg);
    border-radius: 12px;
    padding: 20px;
    border: 1px solid var(--border);
  }
  .mock-header {
    display: flex;
    gap: 6px;
    margin-bottom: 16px;
  }
  .mock-dot { width: 10px; height: 10px; border-radius: 50%; }
  .mock-dot.red { background: #ff5f57; }
  .mock-dot.yellow { background: #febc2e; }
  .mock-dot.green { background: #28c840; }
  .mock-row {
    display: flex;
    align-items: center;
    gap: 12px;
    padding: 10px 0;
    border-bottom: 1px solid var(--border);
    font-size: 13px;
  }
  .mock-row:last-child { border-bottom: none; }
  .mock-avatar {
    width: 28px; height: 28px;
    border-radius: 50%;
    background: linear-gradient(135deg, var(--gradient-start), var(--gradient-end));
    display: flex; align-items: center; justify-content: center;
    font-size: 11px; font-weight: 600; color: #000;
  }
  .mock-name { flex: 1; }
  .mock-tag {
    padding: 3px 10px;
    border-radius: 6px;
    font-size: 11px;
    font-weight: 500;
  }
  .mock-tag.active { background: rgba(52,199,89,0.15); color: var(--success); }
  .mock-tag.pause { background: rgba(255,204,0,0.15); color: #ffcc00; }
  .mock-amount { font-weight: 600; color: var(--accent); }

  .solution-features {
    display: flex;
    flex-direction: column;
    gap: 24px;
  }
  .feature-item {
    display: flex;
    gap: 16px;
    align-items: flex-start;
  }
  .feature-check {
    width: 28px; height: 28px;
    min-width: 28px;
    background: rgba(52,199,89,0.12);
    border-radius: 8px;
    display: flex;
    align-items: center;
    justify-content: center;
    color: var(--success);
    font-size: 14px;
    font-weight: 700;
  }
  .feature-text h4 {
    font-size: 16px;
    font-weight: 600;
    margin-bottom: 4px;
  }
  .feature-text p {
    font-size: 14px;
    color: var(--text-secondary);
    line-height: 1.5;
  }

  /* Benefits */
  .benefits-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
    gap: 24px;
  }
  .benefit-card {
    background: var(--bg-card);
    border: 1px solid var(--border);
    border-radius: 20px;
    padding: 40px 32px;
    position: relative;
    overflow: hidden;
    transition: all 0.4s;
  }
  .benefit-card::after {
    content: '';
    position: absolute;
    bottom: 0; left: 0; right: 0;
    height: 3px;
    background: linear-gradient(90deg, var(--gradient-start), var(--gradient-end));
    transform: scaleX(0);
    transform-origin: left;
    transition: transform 0.4s;
  }
  .benefit-card:hover {
    border-color: var(--border-hover);
    transform: translateY(-6px);
  }
  .benefit-card:hover::after {
    transform: scaleX(1);
  }
  .benefit-num {
    font-family: 'Playfair Display', serif;
    font-size: 48px;
    font-weight: 700;
    color: var(--accent);
    opacity: 0.3;
    line-height: 1;
    margin-bottom: 16px;
  }
  .benefit-card h3 {
    font-size: 19px;
    font-weight: 600;
    margin-bottom: 12px;
  }
  .benefit-card p {
    font-size: 14px;
    color: var(--text-secondary);
    line-height: 1.7;
  }

  /* What's Inside */
  .inside-wrapper {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 16px;
  }
  @media (max-width: 768px) {
    .inside-wrapper { grid-template-columns: 1fr; }
  }
  .inside-item {
    display: flex;
    align-items: center;
    gap: 16px;
    padding: 20px 24px;
    background: var(--bg-card);
    border: 1px solid var(--border);
    border-radius: 12px;
    transition: all 0.3s;
  }
  .inside-item:hover {
    border-color: var(--accent);
    background: var(--bg-elevated);
  }
  .inside-icon {
    width: 44px; height: 44px;
    min-width: 44px;
    background: var(--accent-glow);
    border-radius: 10px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 20px;
  }
  .inside-text h4 {
    font-size: 15px;
    font-weight: 600;
    margin-bottom: 2px;
  }
  .inside-text p {
    font-size: 13px;
    color: var(--text-muted);
  }

  /* Pricing */
  .pricing-card {
    max-width: 480px;
    margin: 0 auto;
    background: var(--bg-card);
    border: 1px solid var(--border);
    border-radius: 24px;
    padding: 48px 40px;
    text-align: center;
    position: relative;
    overflow: hidden;
  }
  .pricing-card::before {
    content: '';
    position: absolute;
    top: 0; left: 0; right: 0;
    height: 4px;
    background: linear-gradient(90deg, var(--gradient-start), var(--gradient-end));
  }
  .pricing-badge {
    display: inline-block;
    padding: 6px 16px;
    background: var(--accent-glow);
    color: var(--accent);
    border-radius: 100px;
    font-size: 12px;
    font-weight: 600;
    text-transform: uppercase;
    letter-spacing: 1px;
    margin-bottom: 24px;
  }
  .pricing-name {
    font-family: 'Playfair Display', serif;
    font-size: 28px;
    font-weight: 700;
    margin-bottom: 8px;
  }
  .pricing-desc {
    color: var(--text-secondary);
    font-size: 15px;
    margin-bottom: 32px;
  }
  .pricing-price {
    display: flex;
    align-items: baseline;
    justify-content: center;
    gap: 8px;
    margin-bottom: 8px;
  }
  .price-old {
    font-size: 24px;
    color: var(--text-muted);
    text-decoration: line-through;
  }
  .price-current {
    font-size: 56px;
    font-weight: 800;
    background: linear-gradient(135deg, var(--gradient-start), var(--gradient-end));
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    line-height: 1;
  }
  .price-currency {
    font-size: 20px;
    color: var(--text-secondary);
    font-weight: 500;
  }
  .price-note {
    font-size: 13px;
    color: var(--text-muted);
    margin-bottom: 32px;
  }
  .pricing-features {
    text-align: left;
    margin-bottom: 32px;
  }
  .pricing-features li {
    list-style: none;
    padding: 10px 0;
    border-bottom: 1px solid var(--border);
    font-size: 14px;
    display: flex;
    align-items: center;
    gap: 10px;
  }
  .pricing-features li::before {
    content: '✓';
    color: var(--success);
    font-weight: 700;
    font-size: 14px;
  }
  .pricing-features li:last-child { border-bottom: none; }
  .pricing-btn {
    width: 100%;
    padding: 18px;
    font-size: 16px;
    font-weight: 600;
  }
  .pricing-guarantee {
    margin-top: 20px;
    font-size: 12px;
    color: var(--text-muted);
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 6px;
  }

  /* FAQ */
  .faq-item {
    background: var(--bg-card);
    border: 1px solid var(--border);
    border-radius: 12px;
    margin-bottom: 12px;
    overflow: hidden;
    transition: all 0.3s;
  }
  .faq-item:hover { border-color: var(--border-hover); }
  .faq-question {
    padding: 20px 24px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    cursor: pointer;
    font-weight: 500;
    font-size: 15px;
  }
  .faq-question::after {
    content: '+';
    font-size: 20px;
    color: var(--accent);
    transition: transform 0.3s;
  }
  .faq-item.active .faq-question::after {
    transform: rotate(45deg);
  }
  .faq-answer {
    max-height: 0;
    overflow: hidden;
    transition: max-height 0.4s ease, padding 0.4s ease;
    padding: 0 24px;
    color: var(--text-secondary);
    font-size: 14px;
    line-height: 1.7;
  }
  .faq-item.active .faq-answer {
    max-height: 300px;
    padding: 0 24px 20px;
  }

  /* CTA Section */
  .cta-section {
    text-align: center;
    background: var(--bg-card);
    border-top: 1px solid var(--border);
    border-bottom: 1px solid var(--border);
  }
  .cta-section h2 {
    font-family: 'Playfair Display', serif;
    font-size: clamp(28px, 4vw, 44px);
    font-weight: 700;
    margin-bottom: 16px;
  }
  .cta-section p {
    color: var(--text-secondary);
    font-size: 17px;
    max-width: 480px;
    margin: 0 auto 32px;
  }

  /* Footer */
  footer {
    padding: 40px;
    text-align: center;
    color: var(--text-muted);
    font-size: 13px;
    border-top: 1px solid var(--border);
  }
  footer a {
    color: var(--text-secondary);
    text-decoration: none;
    margin: 0 12px;
  }
  footer a:hover { color: var(--accent); }

  /* Responsive */
  @media (max-width: 768px) {
    nav { padding: 16px 20px; }
    .nav-links { display: none; }
    section { padding: 60px 20px; }
    .hero { padding: 120px 20px 60px; }
    .hero-stats { gap: 24px; }
    .pricing-card { padding: 32px 24px; }
  }
</style>
<base target="_blank">
</head>
<body>

<!-- Navigation -->
<nav>
  <div class="logo">Speech<span>Flow</span></div>
  <div class="nav-links">
    <a href="#problem">Проблема</a>
    <a href="#solution">Решение</a>
    <a href="#benefits">Выгоды</a>
    <a href="#inside">Состав</a>
    <a href="#pricing">Тариф</a>
    <a href="#pricing" class="nav-cta">Купить за 2 490 ₽</a>
  </div>
</nav>

<!-- Hero -->
<section class="hero">
  <div class="hero-bg"></div>
  <div class="hero-content">
    <div class="badge">CRM-система для логопедов</div>
    <h1>
      Перестаньте терять клиентов<br>
      в Excel и блокнотах.<br>
      <span class="highlight">Ведите практику как бизнес.</span>
    </h1>
    <p class="hero-subtitle">
      Готовый шаблон в Notion для частных логопедов: клиенты, расписание, финансы, прогресс — всё в одном месте. Настройка за 10 минут.
    </p>
    <div class="hero-cta-group">
      <a href="#pricing" class="btn-primary">
        Получить доступ — 2 490 ₽
        <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5"><path d="M5 12h14M12 5l7 7-7 7"/></svg>
      </a>
      <a href="#inside" class="btn-secondary">Что внутри</a>
    </div>
    <div class="hero-stats">
      <div class="stat">
        <div class="stat-number">7</div>
        <div class="stat-label">модулей</div>
      </div>
      <div class="stat">
        <div class="stat-number">8</div>
        <div class="stat-label">бонусов</div>
      </div>
      <div class="stat">
        <div class="stat-number">10 мин</div>
        <div class="stat-label">настройка</div>
      </div>
    </div>
  </div>
</section>

<!-- Problem -->
<section id="problem">
  <div class="container">
    <div class="section-header">
      <div class="section-label">Проблема</div>
      <h2 class="section-title">Знакомо?</h2>
      <p class="section-desc">Большинство частных логопедов тонут в рутине вместо того, чтобы развивать практику</p>
    </div>
    <div class="problem-grid">
      <div class="problem-card">
        <div class="problem-icon">😵‍💫</div>
        <h3>Хаос в записях</h3>
        <p>Клиенты разбросаны по WhatsApp, блокнотам и Excel. Забываете, кто когда приходил, что делали, сколько должны.</p>
      </div>
      <div class="problem-card">
        <div class="problem-icon">💸</div>
        <h3>Утечка доходов</h3>
        <p>Не отслеживаете долги, пропускаете оплаты, не знаете реальную прибыль. Работаете «вслепую».</p>
      </div>
      <div class="problem-card">
        <div class="problem-icon">📉</div>
        <h3>Нет системы прогресса</h3>
        <p>Родители не видят динамику, сомневаются в эффективности, уходят к конкурентам после 3–4 занятий.</p>
      </div>
      <div class="problem-card">
        <div class="problem-icon">⏰</div>
        <h3>Расписание в голове</h3>
        <p>Постоянные переписывания, двойные брони, отмены в последний момент. Нервы и репутация страдают.</p>
      </div>
    </div>
  </div>
</section>

<!-- Solution -->
<section id="solution" style="background: var(--bg-card);">
  <div class="container">
    <div class="section-header">
      <div class="section-label">Решение</div>
      <h2 class="section-title">SpeechFlow — вся практика в одном окне</h2>
      <p class="section-desc">Не нужно покупать сложную CRM за 15 000 ₽/мес. Notion + наш шаблон = полный контроль</p>
    </div>
    <div class="solution-wrapper">
      <div class="solution-visual">
        <div class="notion-mock">
          <div class="mock-header">
            <div class="mock-dot red"></div>
            <div class="mock-dot yellow"></div>
            <div class="mock-dot green"></div>
          </div>
          <div style="font-size: 13px; font-weight: 600; margin-bottom: 12px; color: var(--text);">👤 База клиентов</div>
          <div class="mock-row">
            <div class="mock-avatar">АК</div>
            <div class="mock-name">Арсений К., 5 лет</div>
            <div class="mock-tag active">Активный</div>
            <div class="mock-amount">2 500 ₽</div>
          </div>
          <div class="mock-row">
            <div class="mock-avatar">МП</div>
            <div class="mock-name">Мария П., 4 года</div>
            <div class="mock-tag active">Активный</div>
            <div class="mock-amount">2 000 ₽</div>
          </div>
          <div class="mock-row">
            <div class="mock-avatar">ДС</div>
            <div class="mock-name">Даниил С., 6 лет</div>
            <div class="mock-tag pause">На паузе</div>
            <div class="mock-amount">—</div>
          </div>
          <div style="margin-top: 16px; padding-top: 16px; border-top: 1px solid var(--border); display: flex; justify-content: space-between; font-size: 12px; color: var(--text-muted);">
            <span>Активных: 12</span>
            <span>Доход: 28 500 ₽/мес</span>
          </div>
        </div>
      </div>
      <div class="solution-features">
        <div class="feature-item">
          <div class="feature-check">✓</div>
          <div class="feature-text">
            <h4>База клиентов с историей</h4>
            <p>Все данные ребенка, родителей, диагнозы, цели — структурировано и всегда под рукой</p>
          </div>
        </div>
        <div class="feature-item">
          <div class="feature-check">✓</div>
          <div class="feature-text">
            <h4>Умное расписание</h4>
            <p>Календарь занятий, автоматические напоминания, учет отмен и переносов</p>
          </div>
        </div>
        <div class="feature-item">
          <div class="feature-check">✓</div>
          <div class="feature-text">
            <h4>Финансовый трекер</h4>
            <p>Доходы, расходы, долги, налоги — автоматические формулы считают всё сами</p>
          </div>
        </div>
        <div class="feature-item">
          <div class="feature-check">✓</div>
          <div class="feature-text">
            <h4>Прогресс-панель</h4>
            <p>Наглядная динамика речи, готовые отчеты для родителей, план на месяц</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</section>

<!-- Benefits -->
<section id="benefits">
  <div class="container">
    <div class="section-header">
      <div class="section-label">Выгоды</div>
      <h2 class="section-title">Что изменится за неделю</h2>
      <p class="section-desc">Не просто шаблон — а полная трансформация вашей практики</p>
    </div>
    <div class="benefits-grid">
      <div class="benefit-card">
        <div class="benefit-num">01</div>
        <h3>Экономия 5+ часов в неделю</h3>
        <p>Автоматические отчеты, шаблоны сообщений родителям, готовые материалы — больше не пишете одно и тоё с нуля. Время на новых клиентов или отдых.</p>
      </div>
      <div class="benefit-card">
        <div class="benefit-num">02</div>
        <h3>Родители видят результат</h3>
        <p>Ежемесячные отчеты с графиками прогресса повышают доверие. Родители понимают ценность и продлевают курсы в 2 раза чаще.</p>
      </div>
      <div class="benefit-card">
        <div class="benefit-num">03</div>
        <h3>Ни одного упущенного платежа</h3>
        <p>Система долгов и предоплат работает автоматически. Знаете, кто сколько должен, до того как родитель сам вспомнит об этом.</p>
      </div>
      <div class="benefit-card">
        <div class="benefit-num">04</div>
        <h3>Профессиональный имидж</h3>
        <p>Когда у вас есть CRM, договоры, отчеты — вы воспринимаетесь как серьезный специалист, а не «та женщина с объявления».</p>
      </div>
      <div class="benefit-card">
        <div class="benefit-num">05</div>
        <h3>Масштабирование без боли</h3>
        <p>Добавляете ассистента или второго логопеда? Вся база, материалы и процессы уже структурированы. Не начинаете с нуля.</p>
      </div>
      <div class="benefit-card">
        <div class="benefit-num">06</div>
        <h3>Работает на бесплатном Notion</h3>
        <p>Не нужно платить 500–1500 ₽/мес за подписку. Весь функционал доступен на бесплатном тарифе Notion.</p>
      </div>
    </div>
  </div>
</section>

<!-- What's Inside -->
<section id="inside" style="background: var(--bg-card);">
  <div class="container">
    <div class="section-header">
      <div class="section-label">Состав</div>
      <h2 class="section-title">7 модулей + 8 бонусов</h2>
      <p class="section-desc">Всё, что нужно для управления практикой — от первого клиента до расширения команды</p>
    </div>
    <div class="inside-wrapper">
      <div class="inside-item">
        <div class="inside-icon">👤</div>
        <div class="inside-text">
          <h4>База клиентов</h4>
          <p>Карточки с историей, диагнозами, контактами</p>
        </div>
      </div>
      <div class="inside-item">
        <div class="inside-icon">📅</div>
        <div class="inside-text">
          <h4>Расписание занятий</h4>
          <p>Календарь, статусы, напоминания</p>
        </div>
      </div>
      <div class="inside-item">
        <div class="inside-icon">💰</div>
        <div class="inside-text">
          <h4>Финансовый трекер</h4>
          <p>Доходы, расходы, долги, аналитика</p>
        </div>
      </div>
      <div class="inside-item">
        <div class="inside-icon">📊</div>
        <div class="inside-text">
          <h4>Прогресс-панель</h4>
          <p>Графики, отчеты для родителей</p>
        </div>
      </div>
      <div class="inside-item">
        <div class="inside-icon">📁</div>
        <div class="inside-text">
          <h4>Библиотека материалов</h4>
          <p>50+ упражнений, карточки, игры</p>
        </div>
      </div>
      <div class="inside-item">
        <div class="inside-icon">🎯</div>
        <div class="inside-text">
          <h4>Цели и задачи</h4>
          <p>Планирование, дедлайны, приоритеты</p>
        </div>
      </div>
      <div class="inside-item">
        <div class="inside-icon">📈</div>
        <div class="inside-text">
          <h4>Аналитика</h4>
          <p>Автоматические дашборды и метрики</p>
        </div>
      </div>
      <div class="inside-item">
        <div class="inside-icon">🎁</div>
        <div class="inside-text">
          <h4>8 бонусов</h4>
          <p>Договоры, шаблоны, Canva, видео-гайд</p>
        </div>
      </div>
    </div>
  </div>
</section>

<!-- Pricing -->
<section id="pricing">
  <div class="container">
    <div class="section-header">
      <div class="section-label">Тариф</div>
      <h2 class="section-title">Один платеж — пожизненный доступ</h2>
      <p class="section-desc">Никаких подписок. Обновления шаблона бесплатно навсегда.</p>
    </div>
    <div class="pricing-card">
      <div class="pricing-badge">Популярный выбор</div>
      <div class="pricing-name">SpeechFlow Pro</div>
      <div class="pricing-desc">Полный шаблон + все бонусы + пожизненные обновления</div>
      <div class="pricing-price">
        <span class="price-old">4 990 ₽</span>
        <span class="price-current">2 490</span>
        <span class="price-currency">₽</span>
      </div>
      <div class="price-note">Единоразовый платеж. Доступ сразу после оплаты.</div>
      <ul class="pricing-features">
        <li>7 модулей CRM в Notion</li>
        <li>50+ готовых логопедических упражнений (PDF)</li>
        <li>Шаблон договора с родителями</li>
        <li>Шаблон информированного согласия</li>
        <li>10 Instagram-шаблонов (Canva)</li>
        <li>Чек-лист «Первое занятие»</li>
        <li>Шаблон ежемесячного отчета</li>
        <li>Калькулятор стоимости занятий</li>
        <li>Видео-инструкция по настройке (15 мин)</li>
        <li>Пожизненные обновления шаблона</li>
        <li>Поддержка в Telegram</li>
      </ul>
      <a href="#" class="btn-primary pricing-btn" onclick="alert('Здесь будет ссылка на оплату (Gumroad / CloudPayments / ЮKassa)')">
        Купить сейчас — 2 490 ₽
      </a>
      <div class="pricing-guarantee">
        <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M12 22s8-4 8-10V5l-8-3-8 3v7c0 6 8 10 8 10z"/></svg>
        Гарантия возврата 14 дней, если шаблон не подошел
      </div>
    </div>
  </div>
</section>

<!-- FAQ -->
<section style="background: var(--bg-card);">
  <div class="container">
    <div class="section-header">
      <div class="section-label">FAQ</div>
      <h2 class="section-title">Частые вопросы</h2>
    </div>
    <div class="faq-item">
      <div class="faq-question">Нужен ли платный Notion?</div>
      <div class="faq-answer">Нет. Весь функционал шаблона работает на бесплатном тарифе Notion. Вы просто дублируете шаблон к себе и начинаете работу.</div>
    </div>
    <div class="faq-item">
      <div class="faq-question">Я никогда не работала в Notion. Справлюсь?</div>
      <div class="faq-answer">Да. В комплекте видео-инструкция на 15 минут. Интерфейс интуитивный — большинство логопедов разбираются за 10 минут. Если возникнут вопросы — пишите в поддержку.</div>
    </div>
    <div class="faq-item">
      <div class="faq-question">Можно ли адаптировать под другую специальность?</div>
      <div class="faq-answer">Да, шаблон легко кастомизируется. Многие покупатели адаптируют его под дефектологов, нейропсихологов, психологов и репетиторов.</div>
    </div>
    <div class="faq-item">
      <div class="faq-question">Как получить доступ после оплаты?</div>
      <div class="faq-answer">Сразу после оплаты вы получите ссылку на дублирование шаблона в Notion и доступ к папке с бонусами в Google Drive. Всё автоматически, без ожидания.</div>
    </div>
    <div class="faq-item">
      <div class="faq-question">Будут ли обновления?</div>
      <div class="faq-answer">Да, все обновления шаблона бесплатны и пожизненны. Мы регулярно добавляем новые функции по запросам пользователей.</div>
    </div>
  </div>
</section>

<!-- CTA -->
<section class="cta-section">
  <div class="container">
    <h2>Готовы вести практику как профи?</h2>
    <p>За 10 минут настройки вы получите систему, которую обычно собирают годами. Без подписок, без сложных программ.</p>
    <a href="#" class="btn-primary" onclick="alert('Здесь будет ссылка на оплату (Gumroad / CloudPayments / ЮKassa)')">
      Купить SpeechFlow — 2 490 ₽
      <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5"><path d="M5 12h14M12 5l7 7-7 7"/></svg>
    </a>
  </div>
</section>

<!-- Footer -->
<footer>
  <p>© 2026 SpeechFlow. Все права защищены.</p>
  <p style="margin-top: 8px;">
    <a href="#">Политика конфиденциальности</a>
    <a href="#">Договор-оферта</a>
    <a href="#">Поддержка</a>
  </p>
</footer>

<script>
  // FAQ accordion
  document.querySelectorAll('.faq-question').forEach(q => {
    q.addEventListener('click', () => {
      const item = q.parentElement;
      const isActive = item.classList.contains('active');
      document.querySelectorAll('.faq-item').forEach(i => i.classList.remove('active'));
      if (!isActive) item.classList.add('active');
    });
  });

  // Smooth scroll for nav links
  document.querySelectorAll('a[href^="#"]').forEach(anchor => {
    anchor.addEventListener('click', function(e) {
      e.preventDefault();
      const target = document.querySelector(this.getAttribute('href'));
      if (target) target.scrollIntoView({ behavior: 'smooth', block: 'start' });
    });
  });
</script>

</body>
</html>
