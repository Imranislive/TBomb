<!DOCTYPE html>
<html lang="hi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AI MASTER 2.0</title>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@700&family=Roboto:wght@400;500;700&display=swap" rel="stylesheet">
    <script src="https://checkout.razorpay.com/v1/checkout.js"></script>
    <style>
        :root {
            --primary-gradient: linear-gradient(45deg, #4c68d7, #6a82fb);
            --text-color: #333;
            --bg-color: #f4f6f8;
            --card-bg: #ffffff;
            --shadow: 0 4px 12px rgba(0,0,0,0.08);
            --green-color: #28a745;
            --blue-color: #007bff;
        }
        body { font-family: 'Roboto', sans-serif; margin: 0; background-color: var(--bg-color); color: var(--text-color); }
        .container { padding: 15px; }
        .toolbar { display: flex; align-items: center; justify-content: space-between; background: var(--primary-gradient); padding: 12px 18px; box-shadow: 0 4px 10px rgba(76, 104, 215, 0.3); position: sticky; top: 0; z-index: 100; }
        .toolbar-brand { display: flex; align-items: center; }
        .toolbar img { width: 40px; height: 40px; margin-right: 15px; filter: drop-shadow(0 2px 3px rgba(0,0,0,0.2)); }
        .toolbar h1 { font-family: 'Poppins', sans-serif; font-size: 1.6em; margin: 0; color: #ffffff; text-shadow: 1px 1px 3px rgba(0,0,0,0.2); }
        .user-icon { height: 32px; width: 32px; color: white; cursor: pointer; user-select: none; }
        .search-bar { margin: 15px; }
        .search-bar input { width: 100%; padding: 12px; border: 1px solid #ddd; border-radius: 8px; font-size: 1em; box-sizing: border-box; }
        .grid-view { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; }
        
        @keyframes itemLoadAnimation {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .grid-item { 
            background-color: var(--card-bg); border-radius: 12px; box-shadow: var(--shadow); cursor: pointer; overflow: hidden; position: relative; 
            transition: transform 0.2s ease, box-shadow 0.2s ease;
            opacity: 0; animation: itemLoadAnimation 0.5s ease-out forwards;
        }

        .grid-item:hover { transform: translateY(-5px); box-shadow: 0 6px 16px rgba(0,0,0,0.12); }
        .grid-item img { width: 100%; height: 120px; object-fit: cover; background-color: #eee; }
        .grid-item .content { padding: 10px; }
        .grid-item .title { font-weight: 500; font-size: 1.1em; margin: 0 0 8px 0; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
        .item-footer { display: flex; justify-content: space-between; align-items: center; }
        .item-price-details { display: flex; align-items: center; gap: 8px; }
        .item-price-details .price { font-size: 1.2em; font-weight: 700; }
        .item-price-details .label-inline { background-color: var(--green-color); color: white; padding: 3px 6px; border-radius: 5px; font-size: 0.7em; font-weight: bold; }
        .item-like-container { display: flex; align-items: center; gap: 4px; font-size: 0.9em; color: #666; cursor: pointer; }
        .item-like-container .like-icon { font-size: 1.4em; color: #ff4d4d; }
        .screen { display: none; }
        #main-ui { display: block; }
        #detailsScreen, #downloadsScreen, #complaintScreen { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background-color: var(--bg-color); overflow-y: auto; z-index: 200; }
        .details-header, .downloads-header, .complaint-header { display: flex; align-items: center; padding: 12px 18px; background: var(--primary-gradient); color: white; box-shadow: 0 4px 10px rgba(76, 104, 215, 0.3); }
        .back-icon { font-size: 24px; cursor: pointer; font-weight: bold; margin-right: 15px; }
        .header-title { font-size: 1.4em; font-weight: bold; }
        .horizontal-gallery-container { padding: 20px 0; overflow-x: auto; white-space: nowrap; -webkit-overflow-scrolling: touch; scrollbar-width: none; }
        .horizontal-gallery-container::-webkit-scrollbar { display: none; }
        .gallery-item { display: inline-block; width: 55%; aspect-ratio: 9 / 16; margin-left: 15px; border-radius: 12px; overflow: hidden; box-shadow: 0 4px 12px rgba(0,0,0,0.1); }
        .gallery-item:last-child { margin-right: 15px; }
        .gallery-image { width: 100%; height: 100%; object-fit: cover; }
        .details-content-wrapper { padding: 15px; }
        .details-info-card { background-color: var(--card-bg); padding: 20px; border-radius: 12px; box-shadow: var(--shadow); }
        .details-title { font-family: 'Roboto', sans-serif; font-size: 2em; font-weight: 700; margin: 0 0 15px 0; }
        .actions-row { display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px; }
        .price-tag { display: flex; align-items: center; gap: 10px; font-size: 1.8em; font-weight: 700; }
        .price-tag .label { background-color: var(--green-color); color: white; font-size: 0.5em; padding: 6px 10px; border-radius: 5px; font-weight: bold; }
        .details-icon-group { display: flex; align-items: center; gap: 20px; }
        .like-container, .share-container { text-align: center; cursor: pointer; }
        .details-like-icon, .details-share-icon { font-size: 28px; }
        .details-like-icon { color: #ff4d4d; }
        .details-share-icon { color: #555; }
        .icon-label { font-size: 0.9em; color: #666; }
        .details-description { line-height: 1.6; margin-bottom: 20px; }
        .download-container { background-color: #e9f5ff; padding: 12px; border-radius: 8px; margin-bottom: 15px; text-align: center; }
        .download-link { background-color: var(--blue-color); color: white; padding: 12px 20px; border-radius: 5px; text-decoration: none; font-weight: 500; display: inline-block; }
        .buy-now-btn, .complaint-submit-btn { width: 100%; padding: 15px; background-color: var(--green-color); color: white; border: none; border-radius: 8px; cursor: pointer; font-size: 1.2em; font-weight: bold; }
        .complaint-submit-btn:disabled { background-color: #94d3a2; cursor: not-allowed; }
        .modal { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.6); z-index: 400; justify-content: center; align-items: center; }
        .modal-content { background: var(--card-bg); padding: 25px; border-radius: 12px; width: 90%; max-width: 400px; text-align: center; box-shadow: 0 5px 20px rgba(0,0,0,0.2); }
        .modal-content h2 { font-family: 'Roboto', sans-serif; font-weight: 700; font-size: 1.5em; margin-top: 0; margin-bottom: 25px; }
        
        /* --- UPDATED RULE --- */
        .modal-content input, .form-group select, .form-group textarea, .form-group input { 
            width: 100%; box-sizing: border-box; padding: 12px; border: 1px solid #ccc; border-radius: 8px; font-size: 1em; 
            margin-bottom: 15px; /* This adds space below each input */
        }
        
        .modal-buttons { display: flex; gap: 10px; justify-content: center; }
        .modal-buttons button { flex: 1; padding: 12px; border: none; border-radius: 8px; font-size: 1em; cursor: pointer; font-weight: 500; }
        .btn-submit { background-color: var(--blue-color); color: white; }
        .btn-cancel { background-color: #6c757d; color: white; }
        #sidebar { height: 100%; width: 250px; position: fixed; z-index: 500; top: 0; right: -250px; background-color: var(--card-bg); overflow-x: hidden; transition: 0.3s ease-out; box-shadow: -4px 0 15px rgba(0,0,0,0.1); padding-top: 0; }
        .sidebar-header { background: var(--primary-gradient); color: white; padding: 12px 20px; display: flex; align-items: center; justify-content: space-between; }
        .sidebar-header h2 { margin: 0; font-family: 'Poppins', sans-serif; font-size: 1.4em; }
        #sidebar .closebtn { position: static; font-size: 30px; color: white; text-decoration: none; padding: 0 5px; }
        .sidebar-item { display: flex; align-items: center; padding: 15px 20px; text-decoration: none; font-size: 17px; color: var(--text-color); transition: 0.2s; border-bottom: 1px solid #f0f0f0; }
        .sidebar-item:hover { background-color: #f1f1f1; }
        .sidebar-icon { width: 22px; height: 22px; margin-right: 15px; fill: currentColor; opacity: 0.8; }
        #overlay { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background-color: rgba(0,0,0,0.4); z-index: 499; transition: opacity 0.3s; }
        #downloadsList { padding: 15px; }
        .download-item { display: flex; background-color: var(--card-bg); border-radius: 8px; padding: 10px; margin-bottom: 10px; box-shadow: var(--shadow); align-items: center; }
        .download-item img { width: 80px; height: 80px; object-fit: cover; border-radius: 5px; margin-right: 15px; }
        .download-item-info h4 { margin: 0 0 10px 0; }
        .form-container { padding: 15px; }
        .form-card { background-color: var(--card-bg); padding: 20px; border-radius: 12px; box-shadow: var(--shadow); }
        .form-group { margin-bottom: 20px; }
        .form-group label { display: block; text-align: left; margin-bottom: 8px; font-weight: 500; font-size: 1.1em; }
        .custom-file-upload { border: 1px solid #ccc; display: inline-block; padding: 8px 15px; cursor: pointer; background-color: #f0f0f0; border-radius: 5px; font-weight: 500; }
        input[type="file"] { display: none; }
        #fileNameDisplay { margin-left: 10px; color: #555; font-style: italic; }
    </style>
</head>
<body>
    <div id="main-ui" class="screen">
        <div class="toolbar"><div class="toolbar-brand"><img src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiFnLyOzDs8gngq68zI1_8u7SO4mfC9px6iu7PVw80j34_fn6nDMACws-JUTukEFycCzDtK8SX6re_Sm9glmmpSxHQTRFN-ErgTBRCpY-v1U77FgSv358Etg2sMa5wCyVRIpvAIpMisHyIm82ZWO8uNdOHyl6h41qz3QTirAA-aabErONcKxxUOvYDZRvE/s850/1000166545.png" alt="Icon"><h1>AI MASTER 2.0</h1></div><div class="user-icon" onclick="toggleSidebar()"><svg xmlns="http://www.w3.org/2000/svg" width="100%" height="100%" fill="currentColor" viewBox="0 0 16 16"><path d="M11 6a3 3 0 1 1-6 0 3 3 0 0 1 6 0"/><path fill-rule="evenodd" d="M0 8a8 8 0 1 1 16 0A8 8 0 0 1 0 8m8-7a7 7 0 0 0-5.468 11.37C3.242 11.226 4.805 10 8 10s4.757 1.225 5.468 2.37A7 7 0 0 0 8 1"/></svg></div></div>
        <div class="search-bar"><input type="text" id="searchInput" onkeyup="searchProducts()" placeholder="Search..."></div>
        <div class="container" style="padding-top:0;"><div class="grid-view" id="productGrid"></div></div>
    </div>
    
    <div id="overlay" onclick="toggleSidebar()"></div>
    
    <div id="sidebar">
        <div class="sidebar-header"><h2>Menu</h2><a href="javascript:void(0)" class="closebtn" onclick="toggleSidebar()">&times;</a></div>
        <a href="#" id="myDownloadsLink" class="sidebar-item" onclick="showDownloadsScreen()"><svg class="sidebar-icon" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><path d="M19.35 10.04C18.67 6.59 15.64 4 12 4 9.11 4 6.6 5.64 5.35 8.04 2.34 8.36 0 10.91 0 14c0 3.31 2.69 6 6 6h13c2.76 0 5-2.24 5-5 0-2.64-2.05-4.78-4.65-4.96zM19 18H6c-2.21 0-4-1.79-4-4 0-2.05 1.53-3.76 3.56-3.97l1.07-.11.5-.95C8.08 7.14 9.94 6 12 6c2.62 0 4.88 1.86 5.39 4.43l.3 1.5 1.53.11c1.56.1 2.78 1.44 2.78 3.03 0 1.65-1.35 3-3 3zm-5.55-8h-2.9v3H8l4 4 4-4h-2.55z"/></svg>My Downloads</a>
        <a href="https://t.me/aimasterdpk" target="_blank" class="sidebar-item"><svg class="sidebar-icon" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><path d="M21.933 3.633a.5.5 0 0 0-.626-.445L2.57 9.875a.5.5 0 0 0 .016.929l4.98 1.868 1.868 4.98a.5.5 0 0 0 .929.016l6.688-18.738a.5.5 0 0 0-.444-.626zM4.781 10.491l13.34-5.336-10.237 10.237-2.03-5.414 5.414-2.03L4.78 10.49z"/></svg>Contact Us</a>
        <a href="#" id="addComplaintLink" class="sidebar-item" onclick="showComplaintScreen()"><svg class="sidebar-icon" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor"><path d="M22 4c0-1.1-.9-2-2-2H4c-1.1 0-2 .9-2 2v12c0 1.1.9 2 2 2h14l4 4V4zm-2 13.17L18.83 16H4V4h16v13.17zM11 5h2v6h-2V5zm0 8h2v2h-2v-2z"/></svg>Add Complaint</a>
        <a href="#" id="authLink" class="sidebar-item" onclick="showAuthModal()"><svg class="sidebar-icon" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><path d="M12 12c2.21 0 4-1.79 4-4s-1.79-4-4-4-4 1.79-4 4 1.79 4 4 4zm0 2c-2.67 0-8 1.34-8 4v2h16v-2c0-2.66-5.33-4-8-4z"/></svg>Login / Sign Up</a>
    </div>

    <div id="detailsScreen" class="screen"></div>
    <div id="downloadsScreen" class="screen"></div>
    
    <div id="complaintScreen" class="screen">
        <div class="complaint-header"><div class="back-icon" onclick="goBack()">&#8592;</div><div class="header-title">Register Complaint</div></div>
        <div class="form-container">
            <div class="form-card">
                <p style="text-align: center; margin-top: 0; color: #555;">Please provide details about your issue.</p>
                <form id="complaintForm" onsubmit="event.preventDefault(); submitComplaint();">
                    <div class="form-group"><label for="complaintType">Complaint Type</label><select id="complaintType" required><option value="">Loading types...</option></select></div>
                    <div class="form-group"><label for="complaintPaymentId">Payment ID*</label><input type="text" id="complaintPaymentId" placeholder="e.g., pay_..." required></div>
                    <div class="form-group"><label for="complaintImageFile">Attach Screenshot (Optional)</label><input type="file" id="complaintImageFile" accept="image/*" onchange="updateFileName(this)"><label for="complaintImageFile" class="custom-file-upload">Choose File</label><span id="fileNameDisplay">No file chosen</span></div>
                    <div class="form-group"><label for="complaintMessage">Message</label><textarea id="complaintMessage" rows="5" placeholder="Describe your issue..."></textarea></div>
                    <button type="submit" id="complaintSubmitBtn" class="complaint-submit-btn">Submit Complaint</button>
                </form>
            </div>
        </div>
    </div>
    
    <div id="authModal" class="modal">
        <div class="modal-content"><h2 id="authTitle">Login</h2><input type="email" id="authEmail" placeholder="Email ID" required><input type="password" id="authPassword" placeholder="Password" required><div class="modal-buttons"><button class="btn-submit" id="authSubmitBtn" onclick="handleAuth()">Submit</button><button class="btn-cancel" onclick="closeModal('authModal')">Cancel</button></div><p style="margin-top: 15px;"><a href="#" id="authToggleLink" onclick="toggleAuthMode()">Need an account? Sign Up</a></p></div>
    </div>
    
    <div id="dialogBox" class="modal"></div>

    <script src="https://www.gstatic.com/firebasejs/8.10.1/firebase-app.js"></script>
    <script src="https://www.gstatic.com/firebasejs/8.10.1/firebase-auth.js"></script>
    <script src="https://www.gstatic.com/firebasejs/8.10.1/firebase-database.js"></script>
    <script>
        const firebaseConfig = { apiKey: "AIzaSyC3KRKS9zXXHfVJnYdZSM_prdcxvfN0gL8", authDomain: "aimaster-7ffaf.firebaseapp.com", databaseURL: "https://aimaster-7ffaf-default-rtdb.firebaseio.com", projectId: "aimaster-7ffaf", storageBucket: "aimaster-7ffaf.appspot.com", messagingSenderId: "963488826729", appId: "1:963488826729:web:0c690774cd50209d33a9f8" };
        firebase.initializeApp(firebaseConfig);
        const db = firebase.database();
        const auth = firebase.auth();
        
        let allProducts = [], currentProduct = null, userOrders = [], userProductGrants = {};
        let isAuthModeLogin = true;

        auth.onAuthStateChanged(user => {
            const authLink = document.getElementById('authLink');
            const authIcon = authLink.querySelector('.sidebar-icon');
            if (user) {
                authLink.lastChild.textContent = " Logout";
                authIcon.innerHTML = `<path d="M17 7l-1.41 1.41L18.17 11H8v2h10.17l-2.58 2.58L17 17l5-5zM4 5h8V3H4c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h8v-2H4V5z"/>`;
                authLink.onclick = (e) => { e.preventDefault(); logout(); };
                fetchUserOrders(user.uid);
                fetchUserGrants(user.uid);
            } else {
                authLink.lastChild.textContent = " Login / Sign Up";
                authIcon.innerHTML = `<path d="M12 12c2.21 0 4-1.79 4-4s-1.79-4-4-4-4 1.79-4 4 1.79 4 4 4zm0 2c-2.67 0-8 1.34-8 4v2h16v-2c0-2.66-5.33-4-8-4z"/>`;
                authLink.onclick = (e) => { e.preventDefault(); showAuthModal(); };
                userOrders = [];
                userProductGrants = {};
            }
        });

        const productsRef = db.ref('products');
        productsRef.on('value', (snapshot) => {
            const data = snapshot.val();
            allProducts = data ? Object.keys(data).reverse().map(key => ({ id: key, ...data[key] })) : [];
            const urlParams = new URLSearchParams(window.location.search);
            const productIdFromUrl = urlParams.get('product');
            if (productIdFromUrl && allProducts.some(p => p.id === productIdFromUrl)) { showDetails(productIdFromUrl); } else { renderProducts(allProducts); showScreen('main-ui'); }
        });

        function fetchUserOrders(userId) {
            db.ref('orders').orderByChild('userId').equalTo(userId).on('value', snapshot => {
                userOrders = snapshot.exists() ? Object.values(snapshot.val()) : [];
                if (currentProduct && document.getElementById('detailsScreen').style.display === 'block') { showDetails(currentProduct.id); }
            });
        }
        
        function fetchUserGrants(userId) {
            const grantsRef = db.ref('user_products/' + userId);
            grantsRef.on('value', snapshot => {
                userProductGrants = snapshot.val() || {};
                if (currentProduct && document.getElementById('detailsScreen').style.display === 'block') { showDetails(currentProduct.id); }
            });
        }
        
        function renderProducts(products) {
            const grid = document.getElementById("productGrid");
            if (!grid) return;
            grid.innerHTML = "";
            if (!products || products.length === 0) { grid.innerHTML = "<p>No products found.</p>"; return; }
            const likedProducts = JSON.parse(localStorage.getItem('likedProducts')) || [];
            products.forEach((p, index) => {
                if (!p.coverImage || !p.title) return;
                const isLiked = likedProducts.includes(p.id);
                const likeIcon = isLiked ? '&#10084;' : '&#9825;';
                grid.innerHTML += `<div class="grid-item" onclick="showDetails('${p.id}')" style="animation-delay: ${index * 50}ms"><img src="${p.coverImage}" alt="${p.title}"><div class="content"><div class="title">${p.title}</div><div class="item-footer"><div class="item-price-details"><span class="price">₹${p.price||0}</span>${p.label ? `<span class="label-inline">${p.label}</span>` : ''}</div><div class="item-like-container" onclick="toggleLikeFromGrid(event, '${p.id}')"><span class="like-icon" id="like-icon-${p.id}">${likeIcon}</span><span id="like-count-${p.id}">${p.likes || 0}</span></div></div></div></div>`;
            });
        }

        function showDetails(productId) {
            currentProduct = allProducts.find(p => p.id === productId);
            if (!currentProduct) { goBack(); return; }
            const hasOrder = userOrders.some(order => order.productId === productId);
            const hasGrant = userProductGrants[productId] === true;
            const isPurchased = hasOrder || hasGrant;
            const imag
