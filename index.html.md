<!DOCTYPE html>  
<html lang="en">  
<head>  
<meta charset="UTF-8">  
<meta name="viewport" content="width=device-width, initial-scale=1.0">  
<title>Rajesh Jewellers</title>  
<link rel="manifest" href="manifest.json">  
<meta name="theme-color" content="#bfa136">  
<style>  
  body { font-family: Arial, sans-serif; margin:0; padding:0; background:#f9f9f9; color:#333; }  
  header { background: linear-gradient(90deg, #bfa136, #ffd700); color:white; text-align:center; padding:20px 10px; font-size:28px; font-weight:bold; letter-spacing:1px; }  
  .shop-info { text-align:center; margin:15px 10px; }  
  .shop-info p { margin:5px 0; }  
  .whatsapp-button { display:inline-block; background-color:#25D366; color:white; padding:10px 15px; border-radius:5px; text-decoration:none; font-weight:bold; margin:10px 0; }  
  .rates-table { width:90%; margin:20px auto; border-collapse:collapse; box-shadow:0 0 10px rgba(0,0,0,0.1); }  
  .rates-table th, .rates-table td { border:1px solid #ddd; padding:12px; text-align:center; }  
  .rates-table th { background-color:#ffd700; color:#333; }  
  .calculator { width:90%; margin:20px auto; padding:15px; background:white; box-shadow:0 0 10px rgba(0,0,0,0.1); border-radius:10px; }  
  .calculator input, .calculator select { width:calc(50% - 20px); padding:10px; margin:5px; border-radius:5px; border:1px solid #ccc; }  
  .calculator button { padding:10px 15px; background-color:#bfa136; color:white; border:none; border-radius:5px; margin:10px 0; cursor:pointer; }  
  .calculator .result { margin-top:10px; font-size:18px; font-weight:bold; }  
  .gallery { display:flex; flex-wrap:wrap; justify-content:center; margin:20px 0; }  
  .gallery img { width:150px; height:150px; margin:10px; border-radius:10px; object-fit:cover; box-shadow:0 0 5px rgba(0,0,0,0.2); }  
  footer { text-align:center; padding:15px 10px; background-color:#333; color:white; }  
</style>  
</head>  
<body>  
  
<header>Rajesh Jewellers</header>  
  
<div class="shop-info">  
  <p>Shop No.6, Near Junior High School, Khajni Bazar, Gorakhpur - 273212</p>  
  <p>Contact: +91 8736831633</p>  
  <a class="whatsapp-button" href="https://wa.me/918736831633" target="_blank">Chat on WhatsApp</a>  
</div>  
  
<table class="rates-table">  
  <thead>  
    <tr>  
      <th>Metal</th>  
      <th>Rate (per 10g)</th>  
    </tr>  
  </thead>  
  <tbody id="rates-body">  
    <tr><td>24K Gold</td><td id="gold24">Loading...</td></tr>  
    <tr><td>22K Gold</td><td id="gold22">Loading...</td></tr>  
    <tr><td>18K Gold</td><td id="gold18">Loading...</td></tr>  
    <tr><td>Silver</td><td id="silver">Loading...</td></tr>  
  </tbody>  
</table>  
  
<div class="calculator">  
  <h3>Gold/Silver Calculator</h3>  
  <input type="number" id="weight" placeholder="Weight (g)">  
  <input type="number" id="making" placeholder="Making Charges (₹)">  
  <input type="number" id="gst" placeholder="GST %">  
  <select id="metal">  
    <option value="gold24">24K Gold</option>  
    <option value="gold22">22K Gold</option>  
    <option value="gold18">18K Gold</option>  
    <option value="silver">Silver</option>  
  </select>  
  <br>  
  <button onclick="calculatePrice()">Calculate</button>  
  <div class="result" id="totalPrice">Total Price: ₹0</div>  
</div>  
  
<div class="gallery">  
  <img src="https://images.unsplash.com/photo-1571689938076-9119e0d4e914?crop=entropy&cs=tinysrgb&fit=crop&h=150&w=150" alt="Jewellery 1">  
  <img src="https://images.unsplash.com/photo-1582579936185-289ae3d81a9b?crop=entropy&cs=tinysrgb&fit=crop&h=150&w=150" alt="Jewellery 2">  
  <img src="https://images.unsplash.com/photo-1560347876-aeef00ee58a1?crop=entropy&cs=tinysrgb&fit=crop&h=150&w=150" alt="Jewellery 3">  
  <img src="https://images.unsplash.com/photo-1578432142284-19b73fc8c2d7?crop=entropy&cs=tinysrgb&fit=crop&h=150&w=150" alt="Jewellery 4">  
</div>  
  
<footer>  
  &copy; 2025 Rajesh Jewellers | All Rights Reserved  
</footer>  
  
<script>  
if ('serviceWorker' in navigator) {  
  navigator.serviceWorker.register('sw.js')  
  .then(() => console.log('Service Worker Registered'))  
  .catch(err => console.log('Service Worker registration failed:', err));  
}  
  
async function fetchRates() {  
  try {  
    // Replace with live gold/silver API if available  
    const response = await fetch('https://www.goldapi.io/api/XAU/INR');  
    const data = await response.json();  
    document.getElementById('gold24').innerText = (data.price24 || 119320) + ' ₹';  
    document.getElementById('gold22').innerText = (data.price22 || 108802) + ' ₹';  
    document.getElementById('gold18').innerText = (data.price18 || 89490) + ' ₹';  
    document.getElementById('silver').innerText = (data.silverPrice || 1562) + ' ₹';  
  } catch (err) {  
    document.getElementById('gold24').innerText = '119320 ₹';  
    document.getElementById('gold22').innerText = '108802 ₹';  
    document.getElementById('gold18').innerText = '89490 ₹';  
    document.getElementById('silver').innerText = '1562 ₹';  
  }  
}  
  
function calculatePrice() {  
  const weight = parseFloat(document.getElementById('weight').value) || 0;  
  const making = parseFloat(document.getElementById('making').value) || 0;  
  const gst = parseFloat(document.getElementById('gst').value) || 0;  
  const metal = document.getElementById('metal').value;  
  const rateText = document.getElementById(metal).innerText;  
  const rate = parseFloat(rateText.replace('₹','').trim()) || 0;  
  const base = weight * rate;  
  const total = base + making;  
  const totalWithGST = total + (total * gst / 100);  
  document.getElementById('totalPrice').innerText = 'Total Price: ₹' + totalWithGST.toFixed(2);  
}  
  
fetchRates();  
</script>  
  
</body>  
</html>  
