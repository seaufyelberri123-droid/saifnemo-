<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>SafePay Mall — متجر الدفع الآمن</title>
  <meta name="description" content="SafePay Mall - bilingual affiliate shop (Amazon). Currency auto-detect by country; Formspree checkout." />
  <style>
    /* ---------- Light professional style (white + blue) ---------- */
    :root{
      --bg:#f5f8fb; --surface:#ffffff; --muted:#64748b;
      --brand:#0ea5e9; --brand-2:#2563eb; --accent:#7c3aed;
      --radius:12px; --pad:16px; font-family: Inter, system-ui, Arial, sans-serif;
    }
    *{box-sizing:border-box}
    body{margin:0;background:linear-gradient(180deg,var(--bg),#eef6fb);color:#0b1220;min-height:100vh}
    .wrap{max-width:1150px;margin:18px auto;padding:16px}
    header{display:flex;align-items:center;justify-content:space-between;gap:12px}
    .brand{display:flex;align-items:center;gap:12px}
    .logo{width:56px;height:56px;border-radius:12px;background:linear-gradient(135deg,var(--brand),var(--accent));display:flex;align-items:center;justify-content:center;color:#021025;font-weight:800;font-size:18px}
    h1{margin:0;font-size:18px}
    .sub{color:var(--muted);font-size:13px}
    nav{display:flex;gap:10px;align-items:center}
    button, .btn{background:linear-gradient(90deg,var(--brand),var(--accent));border:none;padding:10px 14px;border-radius:10px;color:#021025;cursor:pointer;font-weight:700}
    .lang{background:transparent;border:1px solid rgba(11,18,32,0.06);padding:8px 10px;border-radius:10px;color:var(--muted);cursor:pointer}
    main{display:grid;grid-template-columns:1fr 340px;gap:18px;margin-top:18px}
    .card{background:var(--surface);padding:14px;border-radius:12px;box-shadow:0 6px 20px rgba(13,24,40,0.06)}
    .hero{display:flex;justify-content:space-between;align-items:center;gap:12px}
    .hero-left h2{margin:0;font-size:20px}
    .hero-left p{margin:6px 0;color:var(--muted)}
    .catalog{display:grid;grid-template-columns:repeat(auto-fit,minmax(220px,1fr));gap:14px;margin-top:12px}
    .product{border-radius:12px;overflow:hidden;background:linear-gradient(180deg,#fff,#fcfeff);padding:12px;border:1px solid rgba(15,23,42,0.03)}
    .product img{width:100%;height:170px;object-fit:cover;border-radius:8px}
    .prod-title{font-weight:700;margin-top:8px}
    .prod-desc{color:var(--muted);font-size:13px;margin-top:6px}
    .prod-row{display:flex;justify-content:space-between;align-items:center;margin-top:10px}
    .price{font-weight:800;color:var(--brand-2)}
    aside{position:sticky;top:18px}
    .cart-list{display:flex;flex-direction:column;gap:8px;margin-top:8px}
    .cart-item{display:flex;gap:10px;align-items:center}
    .cart-item img{width:56px;height:44px;border-radius:8px;object-fit:cover}
    .muted{color:var(--muted);font-size:13px}
    footer{text-align:center;margin-top:18px;color:var(--muted);font-size:13px}
    /* overlay modal checkout */
    .overlay{position:fixed;inset:0;background:rgba(2,6,23,0.45);display:none;align-items:center;justify-content:center;padding:18px;z-index:120}
    .modal{background:var(--surface);color:#072032;padding:18px;border-radius:12px;max-width:720px;width:100%;box-shadow:0 10px 40px rgba(2,6,23,0.08)}
    input,textarea,select{padding:10px;border-radius:8px;border:1px solid #e6eef6;width:100%}
    .actions{display:flex;gap:8px;align-items:center;margin-top:12px;flex-wrap:wrap}
    @media (max-width:920px){
      main{grid-template-columns:1fr}
      aside{order:2}
    }
  </style>
</head>
<body>
  <div class="wrap">
    <header>
      <div class="brand">
        <div class="logo">SP</div>
        <div>
          <h1 id="site-name">SafePay Mall</h1>
          <div class="sub" id="site-sub">Secure payments • مدفوعات آمنة</div>
        </div>
      </div>

      <nav>
        <button class="lang" id="lang-toggle">عربي</button>
        <button class="btn" id="open-cart">Cart (<span id="cart-count">0</span>)</button>
      </nav>
    </header>

    <main>
      <section>
        <div class="card hero" id="hero-card">
          <div class="hero-left">
            <h2 id="hero-title">Trusted picks — منتجات موثوقة</h2>
            <p id="hero-sub">Curated Amazon favorites — منتجات مختارة من أمازون</p>
            <div style="margin-top:10px">
              <button class="btn" id="shop-now">Shop Now</button>
              <button class="lang" id="visit-contact">Contact</button>
            </div>
          </div>
          <div style="max-width:380px">
            <img id="hero-img" src="https://images.unsplash.com/photo-1503602642458-232111445657?q=80&w=900&auto=format&fit=crop&ixlib=rb-4.0.3&s=8f9ea3d2a6e6a9f0fb8e7c1f0f1b9d34" alt="banner" style="width:100%;border-radius:10px;box-shadow:0 8px 30px rgba(12,22,36,0.06)">
          </div>
        </div>

        <div id="catalog" class="catalog" style="margin-top:14px"></div>
      </section>

      <aside class="card">
        <h3 id="cart-title">Your Cart</h3>
        <div id="cart-empty" class="muted">No items yet</div>
        <div id="cart-list" class="cart-list"></div>

        <div style="margin-top:14px;display:flex;justify-content:space-between;align-items:center">
          <div class="muted">Total</div>
          <div style="font-weight:800" id="cart-total">$0.00</div>
        </div>

        <div style="margin-top:12px">
          <label class="muted">Buy on</label>
          <select id="affiliate-portal" style="width:100%;margin-top:8px;padding:8px;border-radius:8px">
            <option value="amazon.com">Amazon.com (International)</option>
            <option value="amazon.ae">Amazon.ae (UAE)</option>
            <option value="amazon.sa">Amazon.sa (KSA)</option>
            <option value="amazon.eg">Amazon.eg (Egypt)</option>
          </select>
        </div>

        <div style="margin-top:12px;display:flex;gap:8px">
          <button id="checkout-btn" class="btn" style="flex:1">Checkout / اطلب</button>
          <button id="clear-btn" class="lang">Clear</button>
        </div>

        <hr style="margin:12px 0;border:none;border-top:1px solid rgba(6,20,40,0.04)">
        <div class="muted" style="font-size:13px">Notes:</div>
        <ul class="muted" style="margin-top:8px;line-height:1.4">
          <li>All buy buttons open Amazon product page (affiliate tag placeholder).</li>
          <li>Prices auto-convert to visitor currency (if supported).</li>
        </ul>
      </aside>
    </main>

    <footer>© 2025 SafePay Mall — Designed by Seif</footer>
  </div>

  <!-- Checkout modal -->
  <div class="overlay" id="overlay">
    <div class="modal" role="dialog" aria-modal="true">
      <h2 id="checkout-title">Checkout — إتمام الطلب</h2>
      <p id="checkout-sub" class="muted">Order details will be emailed. You will be redirected to the store for payment.</p>

      <form id="order-form" action="https://formspree.io/f/mzzjvqvd" method="POST">
        <div style="display:flex;gap:8px;flex-wrap:wrap">
          <div style="flex:1;min-width:160px">
            <label id="lbl-name">Full name | الاسم</label>
            <input name="customer_name" id="cust-name" required>
          </div>
          <div style="flex:1;min-width:160px">
            <label id="lbl-email">Email | البريد</label>
            <input type="email" name="customer_email" id="cust-email" required>
          </div>
        </div>

        <div style="margin-top:8px">
          <label id="lbl-phone">Phone | الهاتف</label>
          <input name="customer_phone" id="cust-phone" required>
        </div>

        <div style="margin-top:8px">
          <label id="lbl-address">Address | العنوان</label>
          <textarea name="customer_address" id="cust-address" rows="2" required></textarea>
        </div>

        <div style="margin-top:8px">
          <label>Order notes | ملاحظات</label>
          <input name="customer_notes" id="cust-notes" placeholder="Optional / اختياري">
        </div>

        <input type="hidden" name="order_details" id="order-details">
        <input type="hidden" name="order_total" id="order-total">

        <div class="actions">
          <button type="submit" class="btn" id="place-order">Place order / اطلب الآن</button>
          <button type="button" class="lang" id="btn-proceed-pay" style="display:none">Proceed to payment</button>
          <button type="button" class="lang" id="btn-cancel">Cancel</button>
        </div>

        <p class="muted" style="margin-top:10px" id="payment-note">
          After submission you'll receive order email. Then you'll be redirected to Amazon for purchase.
        </p>
      </form>

    </div>
  </div>

<script>
/* =========================
   CONFIG - Edit these values
   - Put your Amazon affiliate tag in AFFILIATE_TAG
   - Formspree endpoint already set
   ========================= */
const CONFIG = {
  FORM_ENDPOINT: 'https://formspree.io/f/mzzjvqvd',
  AFFILIATE_TAG: 'yourtag-20', // <-- REPLACE with your Amazon affiliate tag (example: mytag-20)
  DEFAULT_CURRENCY: 'USD',
  CURRENCY_BY_COUNTRY: { // currency priority by country code
    'EG': 'EGP', 'SA': 'SAR', 'AE': 'AED', 'US': 'USD', 'GB': 'USD', 'FR':'USD'
  },
  CURRENCY_SYMBOLS: { 'USD':'$', 'EGP':'E£', 'SAR':'﷼', 'AED':'د.إ', 'GBP':'£', 'EUR':'€' }
};

/* =========================
   Sample Amazon products (affiliate links built dynamically)
   Each product has: id, title_en, title_ar, price_usd (base), amazon_asin (for building link)
   Replace asin with real ASINs if you want direct product pages.
   ========================= */
const PRODUCTS = [
  {id:'p1', title_en:'Apple AirPods Pro (2nd Gen)', title_ar:'سماعات Apple AirPods Pro الجيل الثاني', price_usd:199.99, asin:'B0BDH9T8ZV'},
  {id:'p2', title_en:'Samsung Galaxy Watch 5', title_ar:'ساعة سامسونج جالاكسي واتش 5', price_usd:229.99, asin:'B09V3H3M2B'},
  {id:'p3', title_en:'Logitech MX Master 3 Mouse', title_ar:'ماوس Logitech MX Master 3', price_usd:99.99, asin:'B07S395RWD'},
  {id:'p4', title_en:'Kindle Paperwhite', title_ar:'قارئ Kindle Paperwhite', price_usd:139.99, asin:'B08KTZ8249'},
  {id:'p5', title_en:'Anker Portable Charger 20000mAh', title_ar:'شاحن محمول Anker 20000mAh', price_usd:49.99, asin:'B07X5JX2F8'},
  {id:'p6', title_en:'Instant Pot Duo 7-in-1', title_ar:'طباخ Instant Pot Duo 7 في 1', price_usd:89.99, asin:'B08PQ2KWHS'}
];

/* =========================
   State
   ========================= */
let LANG = 'en';
let USER_COUNTRY = null;
let USER_CURRENCY = CONFIG.DEFAULT_CURRENCY;
let RATES = null; // will hold conversion rates (base USD)
let CART = JSON.parse(localStorage.getItem('safepay_aff_cart_v1')||'[]');

const $ = id => document.getElementById(id);
const fmt = (val, cur) => {
  const symbol = CONFIG.CURRENCY_SYMBOLS[cur] || cur + ' ';
  if(cur === 'EGP') return symbol + Number(val).toFixed(2);
  return symbol + Number(val).toFixed(2);
};

/* =========================
   Utilities: Build Amazon link per region
   - portal: 'amazon.com', 'amazon.ae', 'amazon.sa', 'amazon.eg'
   - asin: product ASIN
   - tag: affiliate tag
   Note: For many countries Amazon uses different domains; affiliate tags differ per locale.
   Keep tag general or replace per domain when you register.
   ========================= */
function buildAmazonLink(asin, portal, tag){
  // Simple: open product by ASIN on portal
  if(!portal) portal = 'amazon.com';
  // For amazon.eg, amazon.sa, amazon.ae some ASINs may not exist. We still try.
  // Affiliate tag appended as &tag=yourtag-20 for .com; for other locales tag parameter names may differ.
  const base = `https://${portal}/dp/${asin}`;
  // Append tag for .com and .ae/.sa/.eg (many programs accept tag param)
  const sep = base.includes('?') ? '&' : '?';
  return base + sep + 'tag=' + encodeURIComponent(tag);
}

/* =========================
   Render catalog & cart UI
   ========================= */
function renderCatalog(){
  const catalog = $('catalog'); catalog.innerHTML = '';
  const portal = $('affiliate-portal').value;
  for(const p of PRODUCTS){
    const card = document.createElement('div'); card.className='product';
    const priceInUser = convertPrice(p.price_usd, USER_CURRENCY);
    const priceText = fmt(priceInUser, USER_CURRENCY);
    card.innerHTML = `
      <img src="https://images.unsplash.com/photo-1603791440384-56cd371ee9a7?q=80&w=900&auto=format&fit=crop&ixlib=rb-4.0.3&s=${hashString(p.asin||p.id)}" alt="">
      <div class="prod-title">${LANG==='ar'?p.title_ar:p.title_en}</div>
      <div class="prod-desc">${LANG==='ar'?p.title_ar:p.title_en}</div>
      <div class="prod-row">
        <div class="price">${priceText}</div>
        <div style="display:flex;gap:8px">
          <button class="btn add-btn" data-id="${p.id}">${LANG==='ar'?'أضف للسلة':'Add to cart'}</button>
          <a class="lang" href="${buildAmazonLink(p.asin, portal, CONFIG.AFFILIATE_TAG)}" target="_blank" rel="noopener">Buy</a>
        </div>
      </div>
    `;
    catalog.appendChild(card);
  }
  document.querySelectorAll('.add-btn').forEach(b => b.onclick = () => addToCart(b.dataset.id));
  updateCartUI();
}

/* hash to create image seed (stable) */
function hashString(s){
  let h=0; for(let i=0;i<s.length;i++){ h = ((h<<5)-h)+s.charCodeAt(i); h |= 0; } return Math.abs(h);
}

/* CART */
function saveCart(){ localStorage.setItem('safepay_aff_cart_v1', JSON.stringify(CART)); }
function addToCart(id){
  const p = PRODUCTS.find(x=>x.id===id);
  let it = CART.find(x=>x.id===id);
  if(it) it.qty++; else CART.push({ id:p.id, title_en:p.title_en, title_ar:p.title_ar, price_usd:p.price_usd, qty:1 });
  saveCart(); flashCart(); updateCartUI();
}
function updateCartUI(){
  const list = $('cart-list'); list.innerHTML = '';
  if(CART.length === 0) $('cart-empty').style.display='block'; else $('cart-empty').style.display='none';
  let total = 0;
  CART.forEach(item=>{
    const priceConverted = convertPrice(item.price_usd, USER_CURRENCY);
    total += priceConverted * item.qty;
    const div = document.createElement('div'); div.className='cart-item';
    div.innerHTML = `<img src="https://images.unsplash.com/photo-1603791440384-56cd371ee9a7?q=80&w=900&auto=format&fit=crop&ixlib=rb-4.0.3&s=${hashString(item.id)}">
      <div style="flex:1">
        <div style="font-weight:700">${LANG==='ar'?item.title_ar:item.title_en}</div>
        <div class="muted">${item.qty} × ${fmt(priceConverted, USER_CURRENCY)}</div>
      </div>
      <div style="display:flex;flex-direction:column;gap:6px">
        <button class="lang" data-action="inc" data-id="${item.id}">+</button>
        <button class="lang" data-action="dec" data-id="${item.id}">−</button>
      </div>`;
    list.appendChild(div);
  });
  document.querySelectorAll('button[data-action]').forEach(b=> {
    b.onclick = ()=>{
      const id=b.dataset.id; const act=b.dataset.action; const it=CART.find(x=>x.id===id);
      if(!it) return;
      if(act==='inc') it.qty++; else { it.qty--; if(it.qty<=0) CART = CART.filter(x=>x.id!==id); }
      saveCart(); updateCartUI();
    };
  });
  $('cart-count').innerText = CART.reduce((s,i)=>s+i.qty,0);
  $('cart-total').innerText = fmt(total, USER_CURRENCY);
}

/* convert price using RATES (base USD) */
function convertPrice(amountUSD, targetCurrency){
  if(!RATES || !RATES[targetCurrency]) return amountUSD;
  return amountUSD * RATES[targetCurrency];
}

/* flash cart UI */
function flashCart(){ const b=$('open-cart'); b.animate([{transform:'scale(1)'},{transform:'scale(1.06)'},{transform:'scale(1)'}],{duration:200}); }

/* clear */
function clearCart(){ if(confirm(LANG==='ar'?'مسح السلة؟':'Clear cart?')){ CART=[]; saveCart(); updateCartUI(); } }

/* =========== Geo detect user country & get rates =========== */
async function detectCountryAndRates(){
  try {
    // detect country
    const r = await fetch('https://ipapi.co/json/');
    if(r.ok){
      const info = await r.json();
      USER_COUNTRY = info.country || null;
    }
  } catch(e){
    USER_COUNTRY = null;
  }

  // pick currency from mapping
  if(USER_COUNTRY && CONFIG.CURRENCY_BY_COUNTRY[USER_COUNTRY]) USER_CURRENCY = CONFIG.CURRENCY_BY_COUNTRY[USER_COUNTRY];
  else USER_CURRENCY = CONFIG.DEFAULT_CURRENCY;

  // fetch latest rates from exchangerate.host (base USD)
  try {
    const res = await fetch('https://api.exchangerate.host/latest?base=USD');
    if(res.ok){
      const j = await res.json();
      // rates e.g. j.rates.EGP etc.
      RATES = {};
      // map relevant currencies
      const keys = ['USD','EGP','SAR','AED','GBP','EUR'];
      keys.forEach(k => { RATES[k] = j.rates[k] || (k==='USD'?1: null); });
      // fallback for EGP might be null in free API — attempt approximate if null:
      if(!RATES['EGP']) RATES['EGP'] = 30.9; // fallback approximate (may vary)
      if(!RATES['SAR']) RATES['SAR'] = 3.75;
      if(!RATES['AED']) RATES['AED'] = 3.67;
    } else {
      RATES = null;
    }
  } catch(e){
    RATES = null;
  }

  // special: if currency not in rates, default to USD conversion 1:1
  if(!RATES) RATES = {'USD':1,'EGP':30.9,'SAR':3.75,'AED':3.67,'GBP':1,'EUR':1};

  // Language auto: Egyptian visitors -> Arabic (Egyptian), others -> English (but Arabic for Arabic countries)
  if(USER_COUNTRY === 'EG') setLang('ar');
  else if(['SA','AE','KW','QA','BH','OM','LB','JO','IQ','SY','DZ','MA','TN','LY','SD','YE'].includes(USER_COUNTRY)) setLang('ar');
  else setLang('en');

  renderCatalog();
  updateCartUI();
}

/* ========== Checkout flow ==========
   - On checkout: we open the modal and send order to Formspree (so you have record)
   - Then direct user to Amazon product pages (affiliate) to complete purchase
===================================*/
$('checkout-btn').onclick = ()=>{
  if(CART.length===0){ alert(LANG==='ar' ? 'السلة فارغة' : 'Your cart is empty'); return; }
  $('overlay').style.display = 'flex';
  $('order-details').value = JSON.stringify(CART);
  $('order-total').value = calcCartTotal().toFixed(2);
};
$('btn-cancel').onclick = ()=> $('overlay').style.display = 'none';
$('clear-btn').onclick = clearCart;
$('open-cart').onclick = ()=> { window.scrollTo({top:0,behavior:'smooth'}); flashCart(); };
$('shop-now').onclick = ()=> window.scrollTo({top: $('catalog').offsetTop - 12, behavior:'smooth'});
$('visit-contact').onclick = ()=> { $('overlay').style.display = 'flex'; $('cust-name').focus(); };
$('lang-toggle').onclick = ()=> setLang(LANG === 'en' ? 'ar' : 'en');
$('affiliate-portal').onchange = ()=> renderCatalog();

/* calc total in user currency */
function calcCartTotal(){
  let total = 0;
  for(const item of CART){
    const conv = convertPrice(item.price_usd, USER_CURRENCY);
    total += conv * item.qty;
  }
  return total;
}

/* When order form submitted: send to Formspree then open Amazon links for each item (affiliate) */
$('order-form').addEventListener('submit', async (e) => {
  e.preventDefault();
  const form = e.target;
  const fd = new FormData(form);
  fd.append('order_cart', JSON.stringify(CART));
  fd.append('order_total', calcCartTotal().toFixed(2));
  fd.append('currency', USER_CURRENCY);
  fd.append('country', USER_COUNTRY || 'unknown');

  try {
    const res = await fetch(CONFIG.FORM_ENDPOINT, { method:'POST', body: fd, headers: { 'Accept': 'application/json' }});
    if(!res.ok){ alert(LANG==='ar' ? 'خطأ أثناء إرسال الطلب. حاول لاحقًا.' : 'Error sending order. Try again.'); return; }
  } catch(err){
    alert(LANG==='ar' ? 'لا يوجد اتصال بالإنترنت.' : 'No internet connection.'); return;
  }

  // After saving order, redirect user to Amazon product pages (one by one) OR to chosen portal with search
  // Best user flow: open new tab for each product to its Amazon page with affiliate tag.
  const portal = $('affiliate-portal').value || 'amazon.com';
  // open each product in new tab (user will complete purchase there)
  for(const item of CART){
    const prod = PRODUCTS.find(p => p.id === item.id);
    if(!prod) continue;
    // build URL: attempt /dp/ASIN, fallback to search by title
    let url# saifnemo-
