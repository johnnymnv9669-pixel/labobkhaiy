<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>ร้านค้าออนไลน์</title>
  <style>
    body {
      background: #f5f5f5;
      font-family: Arial;
      color: #333;
      padding: 20px;
    }
    .grid {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
      gap: 20px;
    }
    .product {
      background: white;
      border-radius: 10px;
      padding: 15px;
      box-shadow: 0 0 5px rgba(0,0,0,0.1);
    }
    .product img {
      width: 100%;
      height: 200px;
      object-fit: cover;
      border-radius: 5px;
    }
    .btn {
      background: #ff5722;
      color: white;
      padding: 8px 15px;
      border: none;
      cursor: pointer;
      border-radius: 5px;
    }
    .order-form {
      margin-top: 10px;
    }
    input, textarea {
      width: 100%;
      margin-top: 5px;
      padding: 8px;
      border-radius: 5px;
      border: 1px solid #ccc;
    }
  </style>
</head>
<body>
  <h1>ร้านค้าออนไลน์</h1>
  <div class="grid" id="productGrid"></div>

  <script>
    const scriptURL = 'https://script.google.com/macros/s/AKfycbwlXpuYEHI_s9_3iFAHtfOeDFQQi3ji8WEvejjEVJeXo-8t07GSrasiiY3FF4HjAzXa/exec';

    fetch(scriptURL)
      .then(res => res.json())
      .then(data => {
        const grid = document.getElementById('productGrid');
        data.forEach(item => {
          const card = document.createElement('div');
          card.className = 'product';
          card.innerHTML = `
            <img src="${item.pictures}" alt="${item.Name}">
            <h3>${item.Name}</h3>
            <p>${item.Description}</p>
            <strong>${item.price} ກີບ</strong>
            <form class="order-form" onsubmit="submitOrder(event, '${item.Name}', '${item.price}')">
              <input type="text" name="name" placeholder="ชื่อผู้รับ" required>
              <input type="text" name="phone" placeholder="เบอร์โทร" required>
              <textarea name="address" placeholder="ที่อยู่จัดส่ง" required></textarea>
              <input type="url" name="imageURL" placeholder="แนบลิงก์รูปสลิป (ถ้ามี)">
              <button class="btn">สั่งซื้อ</button>
            </form>
          `;
          grid.appendChild(card);
        });
      });

    function submitOrder(event, product, price) {
      event.preventDefault();
      const form = event.target;
      const data = {
        product,
        price,
        name: form.name.value,
        phone: form.phone.value,
        address: form.address.value,
        imageURL: form.imageURL.value
      };

      fetch(scriptURL, {
        method: 'POST',
        body: JSON.stringify(data),
        headers: { 'Content-Type': 'application/json' }
      })
      .then(res => res.json())
      .then(res => {
        if (res.status === 'success') {
          alert('บันทึกคำสั่งซื้อเรียบร้อย!');
          form.reset();
        }
      });
    }
  </script>
</body>
</html>
