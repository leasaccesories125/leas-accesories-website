<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>My Products</title>
<style>
  body { font-family: Arial, sans-serif; padding: 20px; background: #f7f7f7; }
  #gallery { display: flex; flex-wrap: wrap; gap: 20px; }
  .product { background: #fff; padding: 10px; border-radius: 8px; box-shadow: 0 2px 5px rgba(0,0,0,0.2); width: 200px; text-align: center; }
  .product img { width: 100%; height: 150px; object-fit: cover; border-radius: 5px; }
  .edit-btn { margin-top: 5px; padding: 5px 10px; cursor: pointer; }
</style>
</head>
<body>

<h1>My Products</h1>
<div id="gallery"></div>
<button id="addProductBtn">Add Product</button>
<input type="file" id="imageUpload" style="display:none">

<script>
const gallery = document.getElementById('gallery');
const addProductBtn = document.getElementById('addProductBtn');
const imageUpload = document.getElementById('imageUpload');

const PASSWORD = "MySecret123"; // change to your password
let isLoggedIn = false;

// Load products from localStorage
let products = JSON.parse(localStorage.getItem('products') || '[]');

function saveProducts() {
  localStorage.setItem('products', JSON.stringify(products));
}

function renderProducts() {
  gallery.innerHTML = '';
  products.forEach((product, index) => {
    const div = document.createElement('div');
    div.className = 'product';
    div.innerHTML = `
      <img src="${product.img}" alt="${product.name}">
      <h3>${product.name}</h3>
      <p>${product.price}</p>
      ${isLoggedIn ? '<button class="edit-btn">Edit</button>' : ''}
    `;
    if(isLoggedIn) {
      div.querySelector('.edit-btn').addEventListener('click', () => editProduct(index));
    }
    gallery.appendChild(div);
  });
}

function addProduct(name="New Product", price="$0", img="https://via.placeholder.com/200") {
  products.push({name, price, img});
  saveProducts();
  renderProducts();
}

function editProduct(index) {
  const product = products[index];
  const newName = prompt("Enter product name:", product.name);
  if(newName) product.name = newName;

  const newPrice = prompt("Enter product price:", product.price);
  if(newPrice) product.price = newPrice;

  imageUpload.onchange = (e) => {
    const file = e.target.files[0];
    if(!file) return;
    const reader = new FileReader();
    reader.onload = function() {
      product.img = reader.result;
      saveProducts();
      renderProducts();
    }
    reader.readAsDataURL(file);
  };
  imageUpload.click();
}

// Button to add product
addProductBtn.addEventListener('click', () => {
  if(!isLoggedIn) {
    const entered = prompt("Enter password to edit products:");
    if(entered === PASSWORD) {
      isLoggedIn = true;
      alert("You can now edit products!");
      renderProducts();
      addProduct();
    } else {
      alert("Wrong password!");
    }
  } else {
    addProduct();
  }
});

// Initial render
renderProducts();
</script>
</body>
</html>
# leas-accesories-website
