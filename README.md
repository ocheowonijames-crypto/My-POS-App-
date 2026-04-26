<!DOCTYPE html>
<html>
<head>
  <title>God's Favour Ventures POS</title>
  <style>
    body{
      font-family:Arial,sans-serif;margin:0;padding:0;min-height:100vh;
      display:flex;justify-content:center;align-items:flex-start;
      background: linear-gradient(135deg,#f5f7fa,#c3cfe2);
    }
    .card,.card-section{
      background: rgba(255,255,255,0.15);
      backdrop-filter: blur(12px);
      border-radius:20px;
      box-shadow:0 8px 32px rgba(0,0,0,0.1);
      padding:20px;
      margin:15px 0;
      transition: transform 0.3s, box-shadow 0.3s;
    }
    .card:hover,.card-section:hover{
      transform: translateY(-5px);
      box-shadow: 0 15px 40px rgba(0,0,0,0.2);
    }
    h2.logo{
      font-size:32px;font-weight:900;
      background: linear-gradient(90deg,#2575fc,#6a11cb);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      text-align:center;
      margin-bottom:5px;
    }
    p.tagline{
      font-style:italic;color:#555;text-align:center;margin-bottom:25px;
    }
    input,select{width:90%;padding:12px;margin:10px 0;border-radius:12px;border:none;outline:none;font-size:14px;background: rgba(255,255,255,0.4);backdrop-filter: blur(6px);transition:0.3s;}
    input:focus,select:focus{background: rgba(255,255,255,0.6);}
    button{width:94%;padding:12px;margin-top:15px;border:none;border-radius:12px;background: linear-gradient(to right,#2575fc,#6a11cb);color:white;font-size:16px;font-weight:bold;cursor:pointer;transition:0.3s;}
    button:hover{transform:translateY(-2px);opacity:0.9;box-shadow:0 8px 25px rgba(0,0,0,0.2);}
    .product-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(180px,1fr));gap:20px;margin-top:10px;}
    .product-card{background: rgba(255,255,255,0.25);border-radius:15px;padding:15px;text-align:center;transition: transform 0.2s, box-shadow 0.2s;}
    .product-card:hover{transform:translateY(-5px);box-shadow:0 10px 30px rgba(0,0,0,0.2);}
    .badge{display:inline-block;padding:5px 12px;border-radius:15px;font-size:12px;color:white;margin-top:5px;font-weight:bold;}
    .badge.Water{background:#00bfff;}
    .badge['Soft Drink']{background:#ff6347;}
    .badge.Snack{background:#32cd32;}
    .badge.Other{background:#ffa500;}
    .badge.Cash{background:#4caf50;}
    .badge['POS Transfer']{background:#ff9800;}
    .filter-pill{display:inline-block;padding:6px 15px;border-radius:20px;margin:5px;cursor:pointer;color:white;font-weight:bold;transition:0.3s;}
    .filter-pill.active{box-shadow:0 5px 15px rgba(0,0,0,0.2);transform:translateY(-2px);}
    .sale-card,.summary-card,.best-card{background: rgba(255,255,255,0.2);border-radius:12px;padding:10px;margin:5px 0;display:flex;justify-content:space-between;align-items:center;font-weight:bold;transition:0.2s;}
    .sale-card:hover,.summary-card:hover,.best-card:hover{transform:translateY(-2px);box-shadow:0 8px 20px rgba(0,0,0,0.2);}
    #total,#historyTotal{font-weight:bold;color:#2575fc;font-size:18px;transition: all 0.5s ease;}
    #sales{max-height:250px;overflow-y:auto;margin-bottom:10px;}
    #dashboard{display:none;width:95%;max-width:1200px;margin-top:20px;}
    section.card-section{margin-top:15px;}
  </style>
</head>
<body>

<!-- LOGIN PAGE -->
<div id="loginPage" class="card">
  <h2 class="logo">God's Favour Ventures</h2>
  <p class="tagline">Empowering daily sales, one product at a time</p>
  <input type="text" id="username" placeholder="Username" required>
  <input type="password" id="password" placeholder="Password" required>
  <button id="loginBtn">Login</button>
</div>

<!-- DASHBOARD -->
<div id="dashboard">
  <h3 style="text-align:center;">Daily Sales Dashboard</h3>
  <button onclick="backupData()">Backup Data</button>
  <div id="filterPills" style="text-align:center;margin-bottom:10px;"></div>

  <section class="card-section">
    <h4>Add / Edit Product</h4>
    <input id="newProductName" placeholder="Product Name">
    <select id="productCategory">
      <option value="Water">Water</option>
      <option value="Soft Drink">Soft Drink</option>
      <option value="Snack">Snack</option>
      <option value="Other">Other</option>
    </select>
    <select id="productType">
      <option value="unit">Bag / Can / Single Unit</option>
      <option value="bundle">Sachet Bundle</option>
    </select>
    <input id="bundleSize" placeholder="Sachets per Sale (if bundle)">
    <input id="newProductPrice" placeholder="Selling Price">
    <input id="newProductCost" placeholder="Cost Price">
    <button onclick="addOrUpdateProduct()">Add / Update Product</button>
  </section>

  <section class="card-section">
    <h4>Products List</h4>
    <div id="productListDisplay" class="product-grid"></div>
  </section>

  <section class="card-section">
    <h4>Sell Product</h4>
    <select id="productList"></select>
    <input id="qty" placeholder="Number of Sachets / Bags / Cans" type="number">
    <select id="payment">
      <option>Cash</option>
      <option>POS Transfer</option>
    </select>
    <button onclick="sell()">Sell</button>
  </section>

  <section class="card-section">
    <h4>Sales Today</h4>
    <div id="sales"></div>
    <h4>Total Sales: ₦<span id="total">0</span></h4>
    <button onclick="clearToday()">Clear Today's Sales</button>
  </section>

  <section class="card-section">
    <h4>Daily Summary</h4>
    <div id="summaryList"></div>
  </section>

  <section class="card-section">
    <h4>Best Selling Products</h4>
    <div id="bestSellersList"></div>
  </section>

  <section class="card-section">
    <h4>Check Past Sales</h4>
    <input type="date" id="historyDate">
    <button onclick="viewHistory()">View Sales</button>
    <div id="historyList"></div>
    <h4>Total That Day: ₦<span id="historyTotal">0</span></h4>
    <input type="file" id="restoreFile">
    <button onclick="restoreData()">Restore Backup</button>
  </section>
</div>

<script>
  // ======= GLOBAL VARIABLES =======
  let total = 0;
  let salesDiv = document.getElementById("sales");
  let qtyInput = document.getElementById("qty");
  let paymentSelect = document.getElementById("payment");
  let productList = document.getElementById("productList");
  let productListDisplay = document.getElementById("productListDisplay");
  let filterPills = document.getElementById("filterPills");
  let editIndex = null;
  let activeFilter = "All";
  const categoryIcons = {"Water":"💧","Soft Drink":"🥤","Snack":"🍪","Other":"📦"};

  // ======= DEFAULT PRODUCTS =======
  if(!localStorage.getItem("products")){
    const defaultProducts = [
      {name:"Cold Water Sachet", category:"Water", type:"bundle", bundle:3, price:100, costPrice:60},
      {name:"Hot Water Sachet", category:"Water", type:"bundle", bundle:4, price:100, costPrice:65},
      {name:"Cold Water Bag", category:"Water", type:"unit", bundle:1, price:500, costPrice:350},
      {name:"Hot Water Bag", category:"Water", type:"unit", bundle:1, price:450, costPrice:320},
      {name:"Coke", category:"Soft Drink", type:"unit", price:300, costPrice:180},
      {name:"Fanta", category:"Soft Drink", type:"unit", price:300, costPrice:180},
      {name:"Malt", category:"Soft Drink", type:"unit", price:500, costPrice:300}
    ];
    localStorage.setItem("products", JSON.stringify(defaultProducts));
  }

  // ======= LOGIN BUTTON =======
  const loginBtn = document.getElementById("loginBtn");
  loginBtn.addEventListener("click", function(){
    const user = document.getElementById("username").value.trim();
    const pass = document.getElementById("password").value.trim();
    let saved = localStorage.getItem("user");
    if(!saved){
      saved = {username:"admin", password:"1234"};
      localStorage.setItem("user", JSON.stringify(saved));
    } else saved = JSON.parse(saved);

    if(user === saved.username && pass === saved.password){
      document.getElementById("loginPage").style.display = "none";
      document.getElementById("dashboard").style.display = "block";
      loadProducts();
      loadToday();
    } else {
      alert("Invalid username or password");
    }
  });

  // ======= LOAD PRODUCTS =======
  function loadProducts(){
    let products = JSON.parse(localStorage.getItem("products"))||[];
    productListDisplay.innerHTML = ""; 
    productList.innerHTML = "";
    products.forEach((p,i)=>{
      if(activeFilter==="All"||p.category===activeFilter){
        productListDisplay.innerHTML+=`
          <div class="product-card">
            <h5>${categoryIcons[p.category]} ${p.name}</h5>
            <span class="badge ${p.category}">${p.category}</span>
            <p>${p.type=="bundle"?p.bundle+" sachets":(p.category==="Water"?"bag(s)":"can(s)")} - ₦${p.price}</p>
            <div><button onclick="editProduct(${i})">Edit</button> <button onclick="deleteProduct(${i})">Delete</button></div>
          </div>`;
        productList.innerHTML+=`<option value="${i}">${p.name}</option>`;
      }
    });
    renderFilterPills();
  }

  function renderFilterPills(){
    const categories=["All","Water","Soft Drink","Snack","Other"];
    filterPills.innerHTML="";
    categories.forEach(cat=>{
      let pill=document.createElement("span");
      pill.innerText=cat;
      pill.className="filter-pill"+(activeFilter===cat?" active":"");
      pill.style.background=cat==="All"?"#6a11cb":(cat==="Water"?"#00bfff":cat==="Soft Drink"?"#ff6347":cat==="Snack"?"#32cd32":"#ffa500");
      pill.onclick=()=>{activeFilter=cat; loadProducts();}
      filterPills.appendChild(pill);
    });
  }

  function addOrUpdateProduct(){
    let name = document.getElementById("newProductName").value;
    let category = document.getElementById("productCategory").value;
    let type = document.getElementById("productType").value;
    let size = parseInt(document.getElementById("bundleSize").value);
    let price = parseInt(document.getElementById("newProductPrice").value);
    let cost = parseInt(document.getElementById("newProductCost").value);
    if(!name||!price||!cost){alert("Enter product name, selling price and cost price"); return;}
    if(type=="bundle" && !size){alert("Enter bundle size"); return;}
    let products=JSON.parse(localStorage.getItem("products"))||[];
    let data={name,category,type,bundle:size||1,price,costPrice:cost};
    if(editIndex!==null){products[editIndex]=data; editIndex=null;} else{products.push(data);}
    localStorage.setItem("products",JSON.stringify(products)); 
    loadProducts(); clearProductForm(); alert("Product saved");
  }

  function clearProductForm(){
    document.getElementById("newProductName").value="";
    document.getElementById("productCategory").value="Water";
    document.getElementById("productType").value="unit";
    document.getElementById("bundleSize").value="";
    document.getElementById("newProductPrice").value="";
    document.getElementById("newProductCost").value="";
  }

  function editProduct(i){
    let p = JSON.parse(localStorage.getItem("products"))[i];
    document.getElementById("newProductName").value = p.name;
    document.getElementById("productCategory").value = p.category;
    document.getElementById("productType").value = p.type;
    document.getElementById("bundleSize").value = p.bundle;
    document.getElementById("newProductPrice").value = p.price;
    document.getElementById("newProductCost").value = p.costPrice;
    editIndex = i;
  }

  function deleteProduct(i){
    if(!confirm("Delete this product?")) return;
    let products = JSON.parse(localStorage.getItem("products"));
    products.splice(i,1);
    localStorage.setItem("products",JSON.stringify(products));
    loadProducts();
  }

  // ======= SELL FUNCTION =======
  function sell(){
    let products = JSON.parse(localStorage.getItem("products"))||[];
    let idx = parseInt(productList.value);
    if(idx<0){alert("Select product"); return;}
    let product = products[idx];
    let quantity = parseInt(qtyInput.value);
    let payType = paymentSelect.value;
    if(!quantity){alert("Enter quantity"); return;}

    let amt = product.type=="bundle" ? Math.ceil(quantity/product.bundle)*product.price : quantity*product.price;
    let unitCost = product.type=="bundle"?product.costPrice/product.bundle:product.costPrice;
    let profit = amt - unitCost*quantity;
    total += amt;
    let timeSold = new Date().toLocaleString();
    let unitLabel = product.type=="bundle"?product.bundle+" sachets":(product.category==="Water"?"bag(s)":"can(s)");
    salesDiv.innerHTML+=`<div class="sale-card">${categoryIcons[product.category]} ${product.name} ${quantity} ${unitLabel} - ₦${amt} <span class="badge ${payType}">${payType}</span> <span style="float:right;font-size:12px;color:#555;">Sold: ${timeSold} | Profit: ₦${profit}</span></div>`;

    let today = new Date().toISOString().slice(0,10);
    let soldData = JSON.parse(localStorage.getItem("soldData_"+today))||{};
    if(!soldData[product.name]) soldData[product.name] = {qty:0,amount:0,profit:0,type:product.type,category:product.category,lastSold:""};
    soldData[product.name].qty += quantity;
    soldData[product.name].amount += amt;
    soldData[product.name].profit += profit;
    soldData[product.name].lastSold = timeSold;
    localStorage.setItem("soldData_"+today, JSON.stringify(soldData));

    document.getElementById("total").innerText = total;
    localStorage.setItem("sales_"+today, salesDiv.innerHTML);
    localStorage.setItem("total_"+today, total);

    qtyInput.value = "";
    loadToday(); // 🔹 refresh summary & best Sellers
  }

  // ======= DAILY SUMMARY =======
  function loadToday(){
  let today = new Date().toISOString().slice(0,10);
  let soldData = JSON.parse(localStorage.getItem("soldData_"+today))||{};
  
  // ===== Daily Summary =====
  let summaryList = document.getElementById("summaryList");
  summaryList.innerHTML = "";
  for(let key in soldData){
    let p = soldData[key];
    let card = document.createElement("div");
    card.className = "summary-card";
    card.innerHTML = `${p.category} ${key}: ${p.qty} sold - ₦${p.amount} | Profit: ₦${p.profit}`;
    summaryList.appendChild(card);
  }

  // ===== Best Sellers (Top 5) =====
  let bestSellersList = document.getElementById("bestSellersList");
  bestSellersList.innerHTML = "";
  let sorted = Object.entries(soldData).sort((a,b)=>b[1].qty - a[1].qty);
  sorted.slice(0,5).forEach(([name, info], index)=>{
    let card = document.createElement("div");
    card.className = "best-card";
    card.style.transition = "transform 0.3s, box-shadow 0.3s";
    card.style.transform = "scale(0.95)";
    card.innerHTML = `${info.category} ${name}: ${info.qty} sold`;
    
    // small animation on new top seller
    setTimeout(()=>{ card.style.transform = "scale(1)"; }, 50 * (index+1));

    bestSellersList.appendChild(card);
  });
}

  // ======= CLEAR TODAY =======
  function clearToday(){
    if(!confirm("Are you sure you want to clear today's sales?")) return;
    const today = new Date().toISOString().slice(0,10);
    localStorage.removeItem("sales_"+today);
    localStorage.removeItem("total_"+today);
    localStorage.removeItem("soldData_"+today);
    total = 0;
    salesDiv.innerHTML = "";
    document.getElementBy
    document.getElementById("total").innerText = total;
    loadToday();
  }

  // ======= VIEW HISTORY =======
  function viewHistory(){
    const date = document.getElementById("historyDate").value;
    if(!date){ alert("Select a date"); return; }
    const salesHTML = localStorage.getItem("sales_" + date) || "No sales data for this date";
    document.getElementById("historyList").innerHTML = salesHTML;
    const totalHistory = localStorage.getItem("total_" + date) || 0;
    document.getElementById("historyTotal").innerText = totalHistory;
  }

  // ======= BACKUP DATA =======
  function backupData(){
    const products = localStorage.getItem("products");
    const today = new Date().toISOString().slice(0,10);
    const sales = localStorage.getItem("sales_" + today);
    const totalSales = localStorage.getItem("total_" + today);
    const soldData = localStorage.getItem("soldData_" + today);

    const backup = {
      products: JSON.parse(products || "[]"),
      sales: sales || "",
      total: parseInt(totalSales || "0"),
      soldData: JSON.parse(soldData || "{}")
    };

    const dataStr = "data:text/json;charset=utf-8," + encodeURIComponent(JSON.stringify(backup));
    const dlAnchor = document.createElement('a');
    dlAnchor.setAttribute("href", dataStr);
    dlAnchor.setAttribute("download", "pos_backup_" + today + ".json");
    dlAnchor.click();
  }

  // ======= RESTORE DATA =======
  function restoreData(){
    const fileInput = document.getElementById("restoreFile");
    if(fileInput.files.length === 0){ alert("Select a backup file"); return; }

    const reader = new FileReader();
    reader.onload = function(e){
      try {
        const data = JSON.parse(e.target.result);
        localStorage.setItem("products", JSON.stringify(data.products || []));
        const today = new Date().toISOString().slice(0,10);
        localStorage.setItem("sales_" + today, data.sales || "");
        localStorage.setItem("total_" + today, data.total || "0");
        localStorage.setItem("soldData_" + today, JSON.stringify(data.soldData || {}));
        alert("Backup restored successfully!");
        loadProducts(); loadToday();
      } catch(err) { alert("Invalid backup file"); }
    }
    reader.readAsText(fileInput.files[0]);
  }
</script>
</body>
</html>
