<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Animated Library Directory</title>
<style>
  /* =========================================
     GLOBAL & SPA VIEW ARCHITECTURE
     ========================================= */
  :root {
    --bg-main: #1a1a1a;
    --accent: rgb(181, 160, 255);
  }
  body {
    margin: 0;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background-color: var(--bg-main);
    color: white;
    overflow-x: hidden;
  }
  .view {
    display: none;
    min-height: 100vh;
    width: 100%;
    opacity: 0;
    transition: opacity 0.5s ease;
  }
  .view.active {
    display: flex;
    opacity: 1;
  }
  .flex-center {
    justify-content: center;
    align-items: center;
    flex-direction: column;
  }

  /* =========================================
     COMPONENT 2: ENTRANCE BUTTON
     ========================================= */
  .light-button button.bt {
    position: relative; height: 200px; display: flex; align-items: flex-end;
    outline: none; background: none; border: none; cursor: pointer;
  }
  .light-button button.bt .button-holder {
    display: flex; flex-direction: column; align-items: center; justify-content: center;
    height: 100px; width: 100px; background-color: #0a0a0a; border-radius: 5px;
    color: #0f0f0f; font-weight: 700; transition: 300ms; outline: #0f0f0f 2px solid; outline-offset: 20px;
  }
  .light-button button.bt .button-holder svg { height: 40px; fill: #0f0f0f; transition: 300ms; margin-bottom: 5px; }
  .light-button button.bt .light-holder {
    position: absolute; height: 200px; width: 100px; display: flex; flex-direction: column; align-items: center;
  }
  .light-button button.bt .light-holder .dot {
    position: absolute; top: 0; width: 10px; height: 10px; background-color: #0a0a0a; border-radius: 10px; z-index: 2;
  }
  .light-button button.bt .light-holder .light {
    position: absolute; top: 0; width: 200px; height: 200px;
    clip-path: polygon(50% 0%, 25% 100%, 75% 100%); background: transparent; pointer-events: none;
  }
  .light-button button.bt:hover .button-holder svg { fill: var(--accent); }
  .light-button button.bt:hover .button-holder { color: var(--accent); outline: var(--accent) 2px solid; outline-offset: 2px; }
  .light-button button.bt:hover .light-holder .light {
    background: linear-gradient(180deg, rgba(181, 160, 255, 0.8) 0%, rgba(255, 255, 255, 0) 75%, rgba(255, 255, 255, 0) 100%);
  }

  /* =========================================
     COMPONENT 3: 3D FLIP LOGIN CARD
     ========================================= */
  .auth-wrapper {
    --input-focus: #2d8cf0; --font-color: #323232; --font-color-sub: #666;
    --bg-color: #fff; --main-color: #323232;
    display: flex; flex-direction: column; align-items: center; justify-content: center;
  }
  .switch { transform: translateY(-30px); position: relative; display: flex; flex-direction: column; justify-content: center; align-items: center; gap: 30px; width: 50px; height: 20px; }
  .card-side::before { position: absolute; content: 'Log in'; left: -70px; top: 0; width: 100px; text-decoration: underline; color: #fff; font-weight: 600; }
  .card-side::after { position: absolute; content: 'Sign up'; left: 70px; top: 0; width: 100px; text-decoration: none; color: #fff; font-weight: 600; }
  .toggle { opacity: 0; width: 0; height: 0; }
  .slider { box-sizing: border-box; border-radius: 5px; border: 2px solid var(--main-color); box-shadow: 4px 4px var(--main-color); position: absolute; cursor: pointer; top: 0; left: 0; right: 0; bottom: 0; background-color: var(--bg-color); transition: 0.3s; }
  .slider:before { box-sizing: border-box; position: absolute; content: ""; height: 20px; width: 20px; border: 2px solid var(--main-color); border-radius: 5px; left: -2px; bottom: 2px; background-color: var(--bg-color); box-shadow: 0 3px 0 var(--main-color); transition: 0.3s; }
  .toggle:checked + .slider { background-color: var(--input-focus); }
  .toggle:checked + .slider:before { transform: translateX(30px); }
  .toggle:checked ~ .card-side:before { text-decoration: none; }
  .toggle:checked ~ .card-side:after { text-decoration: underline; }
  .flip-card__inner { width: 300px; height: 350px; position: relative; background-color: transparent; perspective: 1000px; text-align: center; transition: transform 0.8s; transform-style: preserve-3d; }
  .toggle:checked ~ .flip-card__inner { transform: rotateY(180deg); }
  .toggle:checked ~ .flip-card__front { box-shadow: none; }
  .flip-card__front, .flip-card__back { padding: 20px; position: absolute; display: flex; flex-direction: column; justify-content: center; -webkit-backface-visibility: hidden; backface-visibility: hidden; background: lightgrey; gap: 20px; border-radius: 5px; border: 2px solid var(--main-color); box-shadow: 4px 4px var(--main-color); width: 100%; box-sizing: border-box; }
  .flip-card__back { transform: rotateY(180deg); }
  .flip-card__form { display: flex; flex-direction: column; align-items: center; gap: 20px; }
  .title { margin: 20px 0; font-size: 25px; font-weight: 900; text-align: center; color: var(--main-color); }
  .flip-card__input { width: 220px; height: 40px; border-radius: 5px; border: 2px solid var(--main-color); background-color: var(--bg-color); box-shadow: 4px 4px var(--main-color); font-size: 15px; font-weight: 600; color: var(--font-color); padding: 5px 10px; outline: none; }
  .flip-card__input::placeholder { color: var(--font-color-sub); opacity: 0.8; }
  .flip-card__input:focus { border: 2px solid var(--input-focus); }
  .flip-card__btn { margin: 20px 0; width: 120px; height: 40px; border-radius: 5px; border: 2px solid var(--main-color); background-color: var(--bg-color); box-shadow: 4px 4px var(--main-color); font-size: 17px; font-weight: 600; color: var(--font-color); cursor: pointer; transition: 0.1s; }
  .flip-card__btn:active { box-shadow: 0px 0px var(--main-color); transform: translate(3px, 3px); }

  /* =========================================
     COMPONENT 4: DASHBOARD & SEARCH BAR
     ========================================= */
  .dashboard-header { display: flex; justify-content: center; padding: 40px 20px; perspective: 800px; isolation: isolate; }
  .input__container { position: relative; background: rgba(255, 255, 255, 0.664); padding: 10px 15px; display: flex; justify-content: center; align-items: center; gap: 5px; border-radius: 22px; width: 100%; max-width: 400px; transition: transform 400ms; transform-style: preserve-3d; transform: rotateX(15deg) rotateY(-20deg); }
  .input__container:hover, .input__container:focus-within { transform: rotateX(0deg) rotateY(0deg); }
  .shadow__input { content: ""; position: absolute; width: 100%; height: 100%; left: 0; bottom: 0; z-index: -1; filter: blur(30px); border-radius: 20px; background-color: #999cff; background-image: radial-gradient(at 85% 51%, hsla(60,60%,61%,1) 0px, transparent 50%), radial-gradient(at 74% 68%, hsla(235,69%,77%,1) 0px, transparent 50%), radial-gradient(at 64% 79%, hsla(284,72%,73%,1) 0px, transparent 50%), radial-gradient(at 75% 16%, hsla(283,60%,72%,1) 0px, transparent 50%), radial-gradient(at 90% 65%, hsla(153,70%,64%,1) 0px, transparent 50%), radial-gradient(at 91% 83%, hsla(283,74%,69%,1) 0px, transparent 50%), radial-gradient(at 72% 91%, hsla(213,75%,75%,1) 0px, transparent 50%); }
  .input__button__shadow { cursor: pointer; border: none; background: none; transition: transform 400ms, background 400ms; display: flex; justify-content: center; align-items: center; border-radius: 12px; padding: 5px; }
  .input__button__shadow:hover { background: rgba(255, 255, 255, 0.411); }
  .input__search { width: 100%; border-radius: 20px; outline: none; border: none; padding: 8px; position: relative; background: transparent; color: #17202A; font-weight: bold;}
  .input__search::placeholder { color: #555; }

  /* =========================================
     COMPONENT 1: BOOK CARDS GRID
     ========================================= */
  .book-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(190px, 190px)); gap: 40px; padding: 20px 40px; justify-content: center; max-width: 1200px; margin: 0 auto; }
  .card { width: 190px; height: 254px; border-radius: 30px; background: #212121; box-shadow: 15px 15px 30px rgb(15,15,15), -15px -15px 30px rgb(45,45,45); cursor: pointer; transition: transform 0.3s; position: relative; display: flex; align-items: center; justify-content: center; overflow: hidden; }
  .card:hover { transform: translateY(-10px); }
  .card-placeholder { font-weight: bold; color: #555; font-size: 1.2rem; pointer-events: none;}

  /* =========================================
     COMPONENT 6: BACK TO TOP BUTTON
     ========================================= */
  .back-to-top-container { position: fixed; bottom: 30px; right: 30px; z-index: 1000; display: none; }
  .button { width: 50px; height: 50px; border-radius: 50%; background-color: rgb(20, 20, 20); border: none; font-weight: 600; display: flex; align-items: center; justify-content: center; box-shadow: 0px 0px 0px 4px rgba(180, 160, 255, 0.253); cursor: pointer; transition-duration: 0.3s; overflow: hidden; position: relative; }
  .svgIcon { width: 12px; transition-duration: 0.3s; }
  .svgIcon path { fill: white; }
  .button:hover { width: 140px; border-radius: 50px; transition-duration: 0.3s; background-color: var(--accent); align-items: center; }
  .button:hover .svgIcon { transition-duration: 0.3s; transform: translateY(-200%); }
  .button::before { position: absolute; bottom: -20px; content: "Back to Top"; color: #141414; font-size: 0px; }
  .button:hover::before { font-size: 13px; opacity: 1; bottom: unset; transition-duration: 0.3s; font-weight: bold; }

  /* =========================================
     COMPONENT 5: SPINNER & MODAL (COMPONENT 7)
     ========================================= */
  .modal-overlay { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(10, 10, 10, 0.8); backdrop-filter: blur(5px); display: flex; justify-content: center; align-items: center; z-index: 2000; opacity: 0; visibility: hidden; transition: opacity 0.3s ease; }
  .modal-overlay.active { opacity: 1; visibility: visible; }
  
  /* Spinner styling (unified syntax) */
  .spinner { position: absolute; width: 60px; height: 60px; display: flex; justify-content: center; align-items: center; border-radius: 50%; display: none; }
  .modal-overlay.loading .spinner { display: flex; }
  .modal-overlay.loading .book-modal-content { display: none; }
  .spinner span { position: absolute; top: 50%; width: 35px; height: 7px; background: #ffff; animation: dominos 1s ease infinite; box-shadow: 2px 2px 3px 0px black; }
  .spinner span:nth-child(1) { left: 80px; animation-delay: 0.125s; }
  .spinner span:nth-child(2) { left: 70px; animation-delay: 0.3s; }
  .spinner span:nth-child(3) { left: 60px; animation-delay: 0.425s; }
  .spinner span:nth-child(4) { left: 50px; animation-delay: 0.54s; }
  .spinner span:nth-child(5) { left: 40px; animation-delay: 0.665s; }
  .spinner span:nth-child(6) { left: 30px; animation-delay: 0.79s; }
  .spinner span:nth-child(7) { left: 20px; animation-delay: 0.915s; }
  .spinner span:nth-child(8) { left: 10px; animation-delay: 1.04s; }
  @keyframes dominos { 50% { opacity: 0.7; } 75% { transform: rotate(90deg); } 80% { opacity: 1; } }

  /* Modal Content */
  .book-modal-content { background: #212121; width: 90%; max-width: 700px; border-radius: 30px; padding: 30px; box-shadow: 15px 15px 30px rgb(15,15,15), -15px -15px 30px rgb(45,45,45); transform: translateY(40px) scale(0.95); transition: transform 0.5s cubic-bezier(0.175, 0.885, 0.32, 1.275); position: relative; }
  .modal-overlay.active:not(.loading) .book-modal-content { transform: translateY(0) scale(1); }
  .close-modal { position: absolute; top: 20px; right: 20px; background: none; border: none; fill: #888; width: 32px; height: 32px; cursor: pointer; transition: fill 0.2s, transform 0.2s; }
  .close-modal:hover { fill: #fff; transform: rotate(90deg); }
  .book-modal-layout { display: flex; gap: 30px; flex-wrap: wrap;}
  .book-modal-cover { width: 150px; height: 220px; border-radius: 15px; background: #2a2a2a; flex-shrink: 0; box-shadow: inset 5px 5px 10px #1a1a1a; }
  .book-modal-info { display: flex; flex-direction: column; justify-content: center; flex: 1; }
  .book-modal-title, .book-modal-author, .book-modal-desc, .book-modal-action { opacity: 0; transform: translateY(15px); transition: opacity 0.4s ease, transform 0.4s ease; margin: 5px 0;}
  .modal-overlay.active:not(.loading) .book-modal-title { transition-delay: 0.1s; opacity: 1; transform: translateY(0); font-size: 2rem;}
  .modal-overlay.active:not(.loading) .book-modal-author { transition-delay: 0.2s; opacity: 1; transform: translateY(0); color: var(--accent); font-weight: 600;}
  .modal-overlay.active:not(.loading) .book-modal-desc { transition-delay: 0.3s; opacity: 1; transform: translateY(0); color: #aaa; line-height: 1.6; margin: 15px 0; }
  .modal-overlay.active:not(.loading) .book-modal-action { transition-delay: 0.4s; opacity: 1; transform: translateY(0); }
  .book-modal-action { align-self: flex-start; padding: 12px 30px; border-radius: 20px; border: none; background: var(--accent); color: #141414; font-weight: bold; cursor: pointer; transition: 0.2s; }
  .book-modal-action:hover { transform: scale(1.05); }

</style>
</head>
<body>

  <!-- VIEW 1: ENTRANCE -->
  <div id="view-entrance" class="view active flex-center">
    <div class="light-button">
      <button class="bt" id="enterBtn" aria-label="Enter Library">
        <div class="light-holder">
          <div class="dot"></div>
          <div class="light"></div>
        </div>
        <div class="button-holder">
          <!-- Standardized Login SVG -->
          <svg viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
            <path d="M10 17l5-5-5-5v3H3v4h7v3zm5-13h-3v2h3v12h-3v2h3c1.1 0 2-.9 2-2V6c0-1.1-.9-2-2-2z"/>
          </svg>
          <p>Login</p>
        </div>
      </button>
    </div>
  </div>

  <!-- VIEW 2: AUTHENTICATION -->
  <div id="view-auth" class="view flex-center">
    <div class="auth-wrapper">
      <label class="switch">
         <input type="checkbox" class="toggle">
         <span class="slider"></span>
         <span class="card-side"></span>
         <div class="flip-card__inner">
            <div class="flip-card__front">
               <div class="title">Log in</div>
               <form class="flip-card__form" id="loginForm">
                  <input class="flip-card__input" name="email" placeholder="Email" type="email" required>
                  <input class="flip-card__input" name="password" placeholder="Password" type="password" required>
                  <button class="flip-card__btn" type="submit">Let`s go!</button>
               </form>
            </div>
            <div class="flip-card__back">
               <div class="title">Sign up</div>
               <form class="flip-card__form" id="signupForm">
                  <input class="flip-card__input" name="name" placeholder="Name" type="text" autocomplete="name" required>
                  <input class="flip-card__input" name="email" placeholder="Email" type="email" required>
                  <input class="flip-card__input" name="password" placeholder="Password" type="password" required>
                  <button class="flip-card__btn" type="submit">Confirm!</button>
               </form>
            </div>
         </div>
      </label>
    </div>   
  </div>

  <!-- VIEW 3: MAIN DASHBOARD -->
  <div id="view-dashboard" class="view" style="display: none; flex-direction: column;">
    
    <header class="dashboard-header">
      <div class="input__container">
        <div class="shadow__input"></div>
        <button class="input__button__shadow" aria-label="Search">
          <svg fill="none" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" height="20px" width="20px">
            <path d="M4 9a5 5 0 1110 0A5 5 0 014 9zm5-7a7 7 0 104.2 12.6.999.999 0 00.093.107l3 3a1 1 0 001.414-1.414l-3-3a.999.999 0 00-.107-.093A7 7 0 009 2z" fill-rule="evenodd" fill="#17202A"></path>
          </svg>
        </button>
        <input type="text" name="text" class="input__search" placeholder="What do you want to search?">
      </div>
    </header>

    <main class="book-grid" id="bookGrid">
      <!-- Book Cards generated by JS -->
    </main>

    <!-- BACK TO TOP BUTTON -->
    <div class="back-to-top-container" id="bttContainer">
      <button class="button" id="bttBtn" aria-label="Back to Top">
        <svg class="svgIcon" viewBox="0 0 384 512">
          <path d="M214.6 41.4c-12.5-12.5-32.8-12.5-45.3 0l-160 160c-12.5 12.5-12.5 32.8 0 45.3s32.8 12.5 45.3 0L160 141.2V448c0 17.7 14.3 32 32 32s32-14.3 32-32V141.2L329.4 246.6c12.5 12.5 32.8 12.5 45.3 0s12.5-32.8 0-45.3l-160-160z"></path>
        </svg>
      </button>
    </div>
  </div>

  <!-- GLOBAL OVERLAY: SPINNER & MODAL -->
  <div class="modal-overlay" id="interactiveOverlay">
    
    <!-- Spinner -->
    <div class="spinner" role="status" aria-label="Loading details">
      <span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span>
    </div>

    <!-- Modal Content -->
    <div class="book-modal-content">
      <button class="close-modal" id="closeModalBtn" aria-label="Close details">
        <svg viewBox="0 0 24 24"><path d="M19 6.41L17.59 5 12 10.59 6.41 5 5 6.41 10.59 12 5 17.59 6.41 19 12 13.41 17.59 19 19 17.59 13.41 12z"/></svg>
      </button>
      <div class="book-modal-layout">
        <div class="book-modal-cover"></div> 
        <div class="book-modal-info">
          <h2 class="book-modal-title" id="modalTitle">Book Title</h2>
          <p class="book-modal-author">by Author Name</p>
          <p class="book-modal-desc">Detailed description of the book goes here. This text will appear with a smooth staggered fade-up animation after the loading spinner completes.</p>
          <button class="book-modal-action">Read Now</button>
        </div>
      </div>
    </div>
  </div>

<script>
  // --- STATE MANAGEMENT ---
  const switchView = (hideId, showId) => {
    const hideEl = document.getElementById(hideId);
    const showEl = document.getElementById(showId);
    hideEl.style.opacity = 0;
    setTimeout(() => {
      hideEl.classList.remove('active');
      hideEl.style.display = 'none';
      showEl.style.display = showId === 'view-dashboard' ? 'flex' : 'flex';
      // Slight delay to allow DOM to register display block before fading in
      requestAnimationFrame(() => {
        showEl.classList.add('active');
        showEl.style.opacity = 1;
      });
    }, 500);
  };

  // --- ROUTING LISTENERS ---
  document.getElementById('enterBtn').addEventListener('click', () => switchView('view-entrance', 'view-auth'));

  const handleAuth = (e) => {
    e.preventDefault(); // Prevent page reload
    switchView('view-auth', 'view-dashboard');
    populateBooks();
  };
  document.getElementById('loginForm').addEventListener('submit', handleAuth);
  document.getElementById('signupForm').addEventListener('submit', handleAuth);

  // --- DASHBOARD LOGIC ---
  const populateBooks = () => {
    const grid = document.getElementById('bookGrid');
    if (grid.children.length > 0) return; // Prevent re-rendering
    
    // Generate 12 dummy cards for demonstration
    for (let i = 1; i <= 12; i++) {
      const card = document.createElement('article');
      card.className = 'card';
      card.innerHTML = `<span class="card-placeholder">Book ${i}</span>`;
      card.addEventListener('click', () => openBookModal(`Book Volume ${i}`));
      grid.appendChild(card);
    }
  };

  // Back to Top Logic
  window.addEventListener('scroll', () => {
    const btt = document.getElementById('bttContainer');
    if (window.scrollY > 300) {
      btt.style.display = 'block';
    } else {
      btt.style.display = 'none';
    }
  });

  document.getElementById('bttBtn').addEventListener('click', () => {
    window.scrollTo({ top: 0, behavior: 'smooth' });
  });

  // --- MODAL & SPINNER LOGIC ---
  const overlay = document.getElementById('interactiveOverlay');
  
  const openBookModal = (title) => {
    document.getElementById('modalTitle').innerText = title;
    // 1. Show overlay in Loading state
    overlay.classList.add('active', 'loading');
    
    // 2. Simulate network request, then switch to Modal state
    setTimeout(() => {
      overlay.classList.remove('loading');
    }, 1500); // 1.5 second loading animation
  };

  document.getElementById('closeModalBtn').addEventListener('click', () => {
    overlay.classList.remove('active');
  });

  // Close on outside click
  overlay.addEventListener('click', (e) => {
    if(e.target === overlay) overlay.classList.remove('active');
  });

</script>
</body>
</html>
