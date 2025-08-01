<html lang="th">
<head>
  <meta charset="UTF-8">
  <title>ร้านสนับสนุนบริจาค</title>
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <style>
    body {
      background-color: #304674;
      color: white;
      font-family: sans-serif;
      margin: 0;
      padding: 20px;
    }
    h1 {
      text-align: center;
      margin-bottom: 30px;
    }
    .product-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
      gap: 20px;
    }
    .product {
      background-color: #3f587e;
      border-radius: 8px;
      padding: 15px;
      text-align: center;
      box-shadow: 0 0 10px rgba(0,0,0,0.2);
    }
    .product img {
      width: 100%;
      height: 200px;
      object-fit: cover;
      border-radius: 4px;
      margin-bottom: 10px;
    }
    .product h3 {
      margin: 10px 0 5px;
    }
    .product p {
      margin: 5px 0;
    }
    .buy-button {
      margin-top: 10px;
      padding: 10px 20px;
      background-color: #00cc66;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }
    .buy-button:hover {
      background-color: #00b359;
    }
  </style>
</head>
<body>

<h1>สินค้าสำหรับสนับสนุนการบริจาค</h1>
<div class="product-grid" id="product-list">
  <p>กำลังโหลดสินค้า...</p>
</div>

<script>
  const scriptURL = "https://script.google.com/macros/s/AKfycbzaaXAIygOjPHupnN5pgoOLESeOiX9GNbD2mjop0qJdnQby8SXQBarjo7dtfS8NXsHl/exec";

  async function loadProducts() {
    try {
      const res = await fetch(scriptURL);
      const products = await res.json();

      const container = document.getElementById("product-list");
      container.innerHTML = ""; // เคลียร์ข้อความ "กำลังโหลด"

      if (!Array.isArray(products) || products.length === 0) {
        container.innerHTML = "<p>ไม่พบรายการสินค้า</p>";
        return;
      }

      products.forEach(p => {
        const div = document.createElement("div");
        div.className = "product";
        div.innerHTML = `
          <img src="${p.Image}" alt="${p.Name}">
          <h3>${p.Name}</h3>
          <p>${p.Description || ''}</p>
          <p><strong style="color:#00ff99;">${p.Price} ${p.Currency || 'LAK'}</strong></p>
          <button class="buy-button" onclick="alert('กรุณาติดต่อเพื่อสั่งซื้อ: ${p.Name}')">สั่งซื้อ</button>
        `;
        container.appendChild(div);
      });
    } catch (err) {
      console.error("โหลดสินค้าไม่สำเร็จ:", err);
      document.getElementById("product-list").innerHTML = "<p style='color:red;'>เกิดข้อผิดพลาดในการโหลดข้อมูลสินค้า</p>";
    }
  }

  loadProducts();
</script>

</body>
</html>
