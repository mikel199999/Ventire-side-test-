  
<!DOCTYPE html>  
<html lang="de">  
<head>  
<meta charset="UTF-8">  
<meta name="viewport" content="width=device-width, initial-scale=1.0">  
<title>VENTIRE | Coming Soon</title>  
  
<style>  
* {  
  box-sizing: border-box;  
  margin: 0;  
  padding: 0;  
}  
  
body {  
  background: #080808;  
  color: white;  
  font-family: Arial, sans-serif;  
}  
  
header {  
  display: flex;  
  justify-content: space-between;  
  align-items: center;  
  padding: 25px 6%;  
  border-bottom: 1px solid #252525;  
}  
  
.logo {  
  font-size: 30px;  
  font-weight: 900;  
  letter-spacing: 5px;  
}  
  
button {  
  background: white;  
  color: black;  
  border: none;  
  padding: 12px 20px;  
  cursor: pointer;  
  font-weight: bold;  
}  
  
button:hover {  
  background: #cccccc;  
}  
  
.hero {  
  min-height: 75vh;  
  display: flex;  
  flex-direction: column;  
  align-items: center;  
  justify-content: center;  
  text-align: center;  
  padding: 30px 20px;  
}  
  
.hero h1 {  
  font-size: clamp(55px, 12vw, 130px);  
  letter-spacing: 12px;  
  font-weight: 900;  
}  
  
.hero p {  
  color: #999;  
  margin-top: 20px;  
  letter-spacing: 3px;  
  font-size: 14px;  
}  
  
.line {  
  width: 70px;  
  height: 3px;  
  background: white;  
  margin-top: 30px;  
}  
  
.categories {  
  display: flex;  
  justify-content: center;  
  flex-wrap: wrap;  
  gap: 12px;  
  margin-top: 40px;  
}  
  
.categories span {  
  border: 1px solid #444;  
  padding: 12px 20px;  
  font-size: 12px;  
  letter-spacing: 2px;  
}  
  
.products {  
  display: grid;  
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));  
  gap: 25px;  
  padding: 40px 6%;  
}  
  
.product {  
  background: #141414;  
  padding-bottom: 20px;  
}  
  
.product img {  
  width: 100%;  
  height: 260px;  
  object-fit: cover;  
  background: #222;  
}  
  
.product h3, .product p {  
  margin: 12px 15px;  
}  
  
.product p {  
  color: #aaa;  
}  
  
footer {  
  text-align: center;  
  padding: 30px;  
  color: #666;  
  border-top: 1px solid #252525;  
  font-size: 12px;  
}  
  
#admin {  
  display: none;  
  padding: 30px 6%;  
  background: #111;  
  min-height: 100vh;  
}  
  
#admin input, #admin select {  
  display: block;  
  width: 100%;  
  max-width: 500px;  
  margin: 12px 0;  
  padding: 13px;  
  background: #222;  
  color: white;  
  border: 1px solid #444;  
}  
  
.admin-item {  
  padding: 15px;  
  border: 1px solid #333;  
  margin: 15px 0;  
  max-width: 600px;  
}  
  
.admin-item button {  
  margin: 5px 5px 5px 0;  
}  
  
.hidden {  
  display: none;  
}  
</style>  
</head>  
  
<body>  
  
<header>  
  <div class="logo">VENTIRE</div>  
  <button onclick="login()">ADMIN</button>  
</header>  
  
<section class="hero" id="hero">  
  <h1>VENTIRE</h1>  
  <p id="headline">COMING SOON.</p>  
  <div class="line"></div>  
  
  <div class="categories">  
    <span>STREETWEAR</span>  
    <span>SHOES</span>  
    <span>FRAGRANCE</span>  
    <span>BAGS</span>  
  </div>  
</section>  
  
<section class="products" id="products"></section>  
  
<footer>  
  VENTIRE © 2026 — ALL RIGHTS RESERVED  
</footer>  
  
<section id="admin">  
  <h1>VENTIRE ADMIN</h1>  
  <p>Produktverwaltung</p>  
  <br>  
  
  <button onclick="logout()">ABMELDEN</button>  
  <button onclick="toggleShop()">SHOP ONLINE / OFFLINE</button>  
  
  <hr style="margin:25px 0;border-color:#333">  
  
  <h2>Neues Produkt hinzufügen</h2>  
  
  <input id="name" placeholder="Produktname">  
  <input id="price" type="number" placeholder="Preis in Euro">  
  <input id="image" placeholder="Bild-URL">  
  <input id="stock" type="number" placeholder="Lagerbestand">  
  
  <select id="category">  
    <option>Schuhe</option>  
    <option>Kleidung</option>  
    <option>Parfüm</option>  
    <option>Taschen</option>  
  </select>  
  
  <button onclick="addProduct()">PRODUKT HINZUFÜGEN</button>  
  
  <hr style="margin:25px 0;border-color:#333">  
  
  <h2>Deine Produkte</h2>  
  <div id="adminProducts"></div>  
</section>  
  
<script>  
let products = JSON.parse(  
  localStorage.getItem("ventireProducts") || "[]"  
);  
  
let shopOnline = localStorage.getItem("ventireOnline") === "true";  
  
function save() {  
  localStorage.setItem(  
    "ventireProducts",  
    JSON.stringify(products)  
  );  
  localStorage.setItem("ventireOnline", shopOnline);  
}  
  
function render() {  
  const container = document.getElementById("products");  
  container.innerHTML = "";  
  
  document.getElementById("hero").style.display =  
    shopOnline ? "none" : "flex";  
  
  document.getElementById("headline").textContent =  
    "COMING SOON.";  
  
  if (shopOnline) {  
    products.filter(p => p.visible).forEach(p => {  
      container.innerHTML += `  
        <div class="product">  
          <img src="${p.image}" alt="${p.name}"  
            onerror="this.style.display='none'">  
          <h3>${p.name}</h3>  
          <p>${p.category}</p>  
          <p>${Number(p.price).toFixed(2)} €</p>  
          <p>Bestand: ${p.stock}</p>  
        </div>  
      `;  
    });  
  }  
}  
  
function login() {  
  const password = prompt("Admin-Passwort:");  
  
  if (password === "ventire123") {  
    document.getElementById("admin").style.display = "block";  
    document.body.style.background = "#111";  
    renderAdmin();  
  } else {  
    alert("Falsches Passwort!");  
  }  
}  
  
function logout() {  
  document.getElementById("admin").style.display = "none";  
  document.body.style.background = "#080808";  
}  
  
function addProduct() {  
  const name = document.getElementById("name").value;  
  const price = document.getElementById("price").value;  
  const image = document.getElementById("image").value;  
  const stock = document.getElementById("stock").value;  
  const category = document.getElementById("category").value;  
  
  if (!name || price === "" || Number(price) < 0) {  
    alert("Bitte Produktname und gültigen Preis eingeben.");  
    return;  
  }  
  
  products.push({  
    id: Date.now(),  
    name,  
    price: Number(price),  
    image,  
    stock: Number(stock) || 0,  
    category,  
    visible: false  
  });  
  
  save();  
  renderAdmin();  
  alert("Produkt gespeichert!");  
  
  document.getElementById("name").value = "";  
  document.getElementById("price").value = "";  
  document.getElementById("image").value = "";  
  document.getElementById("stock").value = "";  
}  
  
function renderAdmin() {  
  const container = document.getElementById("adminProducts");  
  container.innerHTML = "";  
  
  products.forEach((p, i) => {  
    container.innerHTML += `  
      <div class="admin-item">  
        <h3>${p.name}</h3>  
        <p>${p.price} € | Bestand: ${p.stock}</p>  
        <p>Status: ${p.visible ? "Online" : "Versteckt"}</p>  
        <button onclick="toggleProduct(${i})">  
          ${p.visible ? "Verstecken" : "Veröffentlichen"}  
        </button>  
        <button onclick="editPrice(${i})">Preis ändern</button>  
        <button onclick="deleteProduct(${i})">Löschen</button>  
      </div>  
    `;  
  });  
}  
  
function toggleProduct(i) {  
  products[i].visible = !products[i].visible;  
  save();  
  renderAdmin();  
  render();  
}  
  
function editPrice(i) {  
  const price = prompt("Neuen Preis in Euro:", products[i].price);  
  
  if (price !== null && price.trim() !== "" &&  
      Number.isFinite(Number(price)) && Number(price) >= 0) {  
    products[i].price = Number(price);  
    save();  
    renderAdmin();  
    render();  
  }  
}  
  
function deleteProduct(i) {  
  if (confirm("Produkt wirklich löschen?")) {  
    products.splice(i, 1);  
    save();  
    renderAdmin();  
    render();  
  }  
}  
  
function toggleShop() {  
  shopOnline = !shopOnline;  
  save();  
  render();  
  alert(shopOnline ? "Shop aktiviert" : "Shop offline");  
}  
  
render();  
</script>  
  
</body>  
</html>  
