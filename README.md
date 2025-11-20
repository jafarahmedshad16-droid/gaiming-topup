# gaiming-topupfrom zipfile import ZipFile

# File content
html_content = """<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>GameTop — Fast Gaming Topup</title>
  <style>
    :root{--accent:#7c3aed;--bg:#0b1020;--card:#0f1724;--muted:#9aa4c0}
    *{box-sizing:border-box}
    body{margin:0;font-family:Inter, ui-sans-serif, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;background:linear-gradient(180deg,#05060a 0%,#071029 100%);color:#e6eef8}
    .container{max-width:1100px;margin:28px auto;padding:18px}
    header{display:flex;align-items:center;justify-content:space-between}
    .brand{display:flex;gap:12px;align-items:center}
    .logo{width:52px;height:52px;border-radius:10px;background:linear-gradient(135deg,var(--accent),#2dd4bf);display:flex;align-items:center;justify-content:center;font-weight:700;color:#051025}
    h1{font-size:20px;margin:0}
    nav a{color:var(--muted);text-decoration:none;margin-left:16px}

    .grid{display:grid;grid-template-columns:1fr 380px;gap:20px;margin-top:22px}

    /* Left */
    .card{background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));padding:18px;border-radius:12px;box-shadow:0 6px 20px rgba(2,6,23,0.6)}
    .hero{display:flex;align-items:center;gap:16px}
    .hero h2{margin:0;font-size:22px}
    .hero p{margin:6px 0 0;color:var(--muted)}

    .services{display:flex;gap:12px;margin-top:14px}
    .service{flex:1;padding:10px;border-radius:10px;background:rgba(255,255,255,0.02);text-align:center}
    .service small{display:block;color:var(--muted)}

    /* products */
    .products{margin-top:18px}
    .products-grid{display:grid;grid-template-columns:repeat(2,1fr);gap:12px}
    .product{display:flex;gap:12px;align-items:center;padding:10px;border-radius:10px;background:rgba(255,255,255,0.015)}
    .product img{width:64px;height:64px;border-radius:8px;object-fit:cover}
    .meta{flex:1}
    .meta h4{margin:0 0 6px 0}
    .meta p{margin:0;color:var(--muted);font-size:13px}
    .btn{background:var(--accent);border:none;padding:8px 12px;border-radius:8px;color:#fff;cursor:pointer}

    /* right (cart) */
    .cart h3{margin-top:0}
    .cart-items{max-height:300px;overflow:auto;margin-bottom:10px}
    .cart-row{display:flex;justify-content:space-between;padding:8px 6px;border-bottom:1px dashed rgba(255,255,255,0.03)}
    .cart-row small{color:var(--muted)}

    .form-row{display:flex;gap:8px;margin-top:8px}
    input,select{width:100%;padding:10px;border-radius:8px;border:1px solid rgba(255,255,255,0.04);background:transparent;color:inherit}

    footer{margin-top:22px;text-align:center;color:var(--muted);font-size:13px}

    /* responsive */
    @media (max-width:900px){.grid{grid-template-columns:1fr} .logo{width:44px;height:44px}}
  </style>
</head>
<body>
  <div class="container">
    <header>
      <div class="brand">
        <div class="logo">GT</div>
        <div>
          <h1>GameTop</h1>
          <div style="color:var(--muted);font-size:13px">Fast, secure game top-ups</div>
        </div>
      </div>
      <nav>
        <a href="#home">Home</a>
        <a href="#topup">Topup</a>
        <a href="#contact">Contact</a>
      </nav>
    </header>

    <main class="grid">
      <section>
        <div class="card hero">
          <div style="flex:1">
            <h2>Instant Game Top-ups — রিয়েল টাইম</h2>
            <p>PUBG, Free Fire, Mobile Legends, Valorant — সবটায় দ্রুত ও নিরাপদ টপ-আপ।</p>

            <div class="services">
              <div class="service">
                <strong>100+</strong>
                <small>সাপোর্টেড গেম</small>
              </div>
              <div class="service">
                <strong>24/7</strong>
                <small>লाइव সাপোর্ট</small>
              </div>
              <div class="service">
                <strong>Secure</strong>
                <small>SSL &amp; Tokenized</small>
              </div>
            </div>

            <div class="products">
              <h3>Popular Topups</h3>
              <div class="products-grid" id="productsGrid">
                <!-- Products inserted by JS -->
              </div>
            </div>
          </div>
          <img src="https://via.placeholder.com/220x140.png?text=GAMING" alt="gaming art" style="border-radius:12px">
        </div>

        <div class="card" style="margin-top:14px">
          <h3 id="topup">Manual Top-up</h3>
          <p style="color:var(--muted);margin-top:6px">নিজে ভর্তি করতে চাও? নিচে গেম, আইডি ও এমাউন্ট বসাও — তারপর Checkout করো।</p>
          <div style="margin-top:10px">
            <label>Game</label>
            <select id="manualGame">
              <option>PUBG Mobile</option>
              <option>Free Fire</option>
              <option>Mobile Legends</option>
              <option>Valorant</option>
            </select>
            <div class="form-row">
              <input id="playerId" placeholder="Player ID / UID" />
              <input id="amount" placeholder="Amount (৳)" />
            </div>
            <div style="margin-top:10px;display:flex;gap:8px">
              <button class="btn" onclick="addManual()">Add to Cart</button>
              <button class="btn" onclick="clearManual()" style="background:transparent;border:1px solid rgba(255,255,255,0.06)">Clear</button>
            </div>
          </div>
        </div>

      </section>

      <aside>
        <div class="card cart">
          <h3>Cart</h3>
          <div class="cart-items" id="cartItems">
            <div style="color:var(--muted);padding:8px">No items yet — add a topup.</div>
          </div>

          <div>
            <div style="display:flex;justify-content:space-between;align-items:center">
              <div style="color:var(--muted);">Subtotal</div>
              <div id="subtotal">৳0</div>
            </div>
            <div style="margin-top:8px">
              <label>Your Email</label>
              <input id="email" placeholder="example@mail.com" />
            </div>
            <div style="margin-top:10px">
              <button class="btn" style="width:100%" onclick="checkout()">Pay with bKash</button>
            </div>
          </div>
        </div>

        <div class="card" style="margin-top:12px">
          <h4>How it works</h4>
          <ol style="color:var(--muted);padding-left:18px">
            <li>Game ও amount নির্বাচন করো</li>
            <li>Checkout দিয়ে payment করো</li>
            <li>Auto delivery within minutes</li>
          </ol>
        </div>
      </aside>
    </main>

    <footer id="contact">
      <div style="max-width:900px;margin:10px auto;padding:12px;background:rgba(255,255,255,0.015);border-radius:10px">Contact: support@gametop.example • Phone: +8801XXXXXXXXX</div>
      <div style="margin-top:8px">© <span id="year"></span> GameTop — All rights reserved</div>
    </footer>
  </div>

  <script>
    const products = [
      {id:1,name:'PUBG UC 300',game:'PUBG Mobile',price:350,desc:'300 UC topup (instant)'} ,
      {id:2,name:'Free Fire Diamond 500',game:'Free Fire',price:420,desc:'500 Diamonds'} ,
      {id:3,name:'ML BB 600',game:'Mobile Legends',price:480,desc:'600 Battle Points'} ,
      {id:4,name:'Valorant Points 1000',game:'Valorant',price:1200,desc:'1000 VP'}
    ];

    const productsGrid = document.getElementById('productsGrid');
    const cartItemsEl = document.getElementById('cartItems');
    const subtotalEl = document.getElementById('subtotal');
    const yearEl = document.getElementById('year');
    yearEl.textContent = new Date().getFullYear();

    let cart = [];

    function renderProducts(){
      products.forEach(p=>{const el = document.createElement('div'); el.className='product';
      el.innerHTML = `<img src="https://via.placeholder.com/120.png?text=${encodeURIComponent(p.name)}" alt="${p.name}" />
          <div class="meta"><h4>${p.name}</h4><p>${p.desc} • ৳${p.price}</p></div>
          <div><button class="btn" onclick='addToCart(${JSON.stringify(p)})'>Buy</button></div>`;
      productsGrid.appendChild(el);})
    }

    function addToCart(product){cart.push({ ...product, qty:1 }); renderCart();}
    function addManual(){const game=document.getElementById('manualGame').value;const id=document.getElementById('playerId').value.trim();const amount=parseFloat(document.getElementById('amount').value);
      if(!id||!amount||isNaN(amount)){alert('Please enter Player ID and valid amount');return;}
      cart.push({id:Date.now(), name:game + ' Topup', game, price: amount, desc: id, qty:1});
      renderCart(); document.getElementById('playerId').value=''; document.getElementById('amount').value='';}
    function clearManual(){document.getElementById('playerId').value='';document.getElementById('amount').value='';}
    function renderCart(){cartItemsEl.innerHTML='';if(cart.length===0){cartItemsEl.innerHTML='<div style="color:var(--muted);padding:8px">No items yet — add a topup.</div>'; subtotalEl.textContent='৳0';return}
      let sum=0;cart.forEach((c,idx)=>{sum+=c.price*c.qty;const row=document.createElement('div');row.className='cart-row';
      row.innerHTML=`<div><strong>${c.name}</strong><br><small>${c.desc}</small></div><div>৳${c.price.toFixed(2)} <button onclick='removeItem(${idx})' style='margin-left:8px;background:transparent;border:none;color:var(--muted);cursor:pointer'>✕</button></div>`;
      cartItemsEl.appendChild(row);});subtotalEl.textContent='৳'+sum.toFixed(2);}
    function removeItem(i){cart.splice(i,1); renderCart();}

    function checkout(){const email=document.getElementById('email').value.trim();if(cart.length===0){alert('Cart is empty');return}if(!email){alert('Please enter your email for receipt');return}
      const payload={cart,email,total:subtotalEl.textContent};console.log('Checkout payload',payload);
      alert('bKash Payment Only!\n\nSend payment to: 1718352014 (Personal).\n\nAfter payment, screenshot পাঠিয়ে দিন।');cart=[];renderCart();}

    renderProducts();

  // --- AI Chat Widget (front-end only) ---
  (function(){const chatHtml=`
    <div id="aiChat" style="position:fixed;right:18px;bottom:18px;z-index:9999;font-family:Inter, system-ui;">
      <button id="chatToggle" style="background:linear-gradient(90deg,var(--accent),#06b6d4);border:none;color:#051025;padding:12px 14px;border-radius:50px;cursor:pointer;box-shadow:0 6px 18px rgba(0,0,0,0.4)">AI Support</button>
      <div id="chatPanel" style="width:320px;max-height:520px;background:linear-gradient(180deg,rgba(10,12,20,0.98),rgba(8,10,18,0.98));border-radius:12px;margin-top:10px;overflow:hidden;display:none;box-shadow:0 10px 30px rgba(2,6,23,0.6)">
        <div style="padding:12px;border-bottom:1px solid rgba(255,255,255,0.03);display:flex;align-items:center;gap:10px"><strong>GameTop AI</strong><small style="color:var(--muted);margin-left:auto">Support Bot</small></div>
        <div id="chatMessages" style="padding:12px;height:360px;overflow:auto;color:var(--muted);font-size:14px"></div>
        <div style="padding:10px;border-top:1px solid rgba(255,255,255,0.02);display:flex;gap:8px">
          <input id="chatInput" placeholder="Write your message..." style="flex:1;padding:10px;border-radius:8px;border:1px solid rgba(255,255,255,0.03);background:transparent;color:inherit" />
          <button id="sendChat" class="btn">Send</button>
        </div>
        <div style="padding:8px;background:rgba(255,255,255,0.01);font-size:12px;color:var(--muted);text-align:center">Note: Requires server endpoint <code>/api/ai-chat</code></div>
      </div>
    </div>`;
    document.body.insertAdjacentHTML('beforeend',chatHtml);
    const chatToggle=document.getElementById('chatToggle');const chatPanel=document.getElementById('chatPanel');const chatMessages=document.getElementById('chatMessages');const chatInput=document.getElementById('chatInput');const sendChat=document.getElementById('sendChat');
    chatToggle.addEventListener('click',()=>{chatPanel.style.display=chatPanel.style.display==='none'?'block':'none';chatMessages.scrollTop=chatMessages.scrollHeight;});
    sendChat.addEventListener('click',sendMessage);
    chatInput.addEventListener('keydown',(e)=>{if(e.key==='Enter')sendMessage();});
    function appendMessage(text,who){const el=document.createElement('div');el.style.marginBottom='10px';if(who==='user')el.innerHTML=`<div style="text-align:right"><div style=\"display:inline-block;background:rgba(124,58,237,0.12);padding:8px 10px;border-radius:8px;color:#e9f2ff\">${escapeHtml(text)}</div></div>`;else el.innerHTML=`<div style=\"display:inline-block;background:rgba(255,255,255,0.02);padding:8px 10px;border-radius:8px;color:var(--muted)\">${escapeHtml(text)}</div>`;chatMessages.appendChild(el);chatMessages.scrollTop=chatMessages.scrollHeight;}
    function escapeHtml(unsafe){return unsafe.replace(/[&<>\"']/g,function(m){return({'&':'&amp;','<':'&lt;','>':'&gt;','\"':'&quot;',\"'\":\"&#039;\"})[m];});}
    async function sendMessage(){const msg=chatInput.value.trim();if(!msg)return;appendMessage(msg,'user');chatInput.value='';appendMessage('Typing...','bot');try{const res=await fetch('/api/ai-chat',{method:'POST',headers:{'Content-Type':'application/json'},body:JSON.stringify({message:msg})});const data=await res.json();if(chatMessages.lastChild)chatMessages.removeChild(chatMessages.lastChild);if(data && data.reply)appendMessage(data.reply,'bot');else appendMessage('No reply (backend returned invalid response).','bot');}catch(err){if(chatMessages.lastChild)chatMessages.removeChild(chatMessages.lastChild);appendMessage('Error contacting AI backend. Configure /api/ai-chat to forward messages to an AI provider.','bot');console.error(err);}}
  })();
</script>
</body>
</html>
"""

# Create zip file
zip_filename = "/mnt/data/GameTop_Website.zip"
with ZipFile(zip_filename, 'w') as zipf:
    zipf.writestr("index.html", html_content)

zip_filename

