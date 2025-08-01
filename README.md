<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Donation Shop</title>
  <style>
    body {
      background: #f0f4f8;
      font-family: 'Segoe UI', sans-serif;
      margin: 0;
      padding: 20px;
    }
    h1 {
      text-align: center;
      color: #304674;
    }
    .grid {
      display: flex;
      flex-wrap: wrap;
      gap: 20px;
      justify-content: center;
    }
    .item {
      background: white;
      border-radius: 8px;
      width: 280px;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
      overflow: hidden;
      text-align: center;
      padding-bottom: 15px;
    }
    .item img {
      width: 100%;
      height: 200px;
      object-fit: cover;
    }
    .item h3 {
      margin: 10px 0 5px;
    }
    .item p {
      margin: 5px 0;
      font-weight: bold;
      color: #27ae60;
    }
    .btn {
      background: #3490dc;
      color: white;
      padding: 8px 16px;
      border: none;
      border-radius: 5px;
      margin-top: 10px;
      cursor: pointer;
    }
    .btn:hover {
      background: #2779bd;
    }
  </style>
</head>
<body>

<h1>🛍️ ระบบขายของเพื่อการบริจาค</h1>
<div class="grid" id="product-list">กำลังโหลดสินค้า...</div>

<script>
const scriptURL = 'https://docs.google.com/spreadsheets/d/1EgDB-c7XdiFhwN2U3OUhVYcB4sTT4Xz_siqUX53qkU4/edit?gid=0#gid=0';

async function loadProducts() {
  try {
    const res = await fetch(scriptURL);
    const products = await res.json();

    const container = document.getElementById('product-list');
    container.innerHTML = products.map(product => `
      <div class="item">
        <img src="${product.image}" alt="${product.name}">
        <h3>${product.name}</h3>
        <p>${product.price} บาท</p>
        <button class="btn" onclick='buy(${JSON.stringify(product)})'>สั่งซื้อ</button>
      </div>
    `).join('');
  } catch (err) {
    document.getElementById('product-list').innerHTML = "❌ โหลดสินค้าล้มเหลว";
    console.error(err);
  }
}

function buy(product) {
  const name = prompt(กรุณาใส่ชื่อของคุณเพื่อสั่งซื้อ "${product.name}");
  if (!name) return;

  fetch(scriptURL, {
    method: 'POST',
    headers: {'Content-Type': 'application/json'},
    body: JSON.stringify({
      name,
      productId: product.id,
      amount: product.price
    })
  })
  .then(res => res.text())
  .then(res => alert("✅ บันทึกคำสั่งซื้อเรียบร้อย!"))
  .catch(err => alert("❌ มีข้อผิดพลาด กรุณาลองใหม่"));
}

loadProducts();
</script>

</body>
</html>
