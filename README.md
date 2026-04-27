<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mini POS - Facturación con imágenes de productos</title>
    <style>
        * {
            box-sizing: border-box;
            font-family: system-ui, 'Segoe UI', monospace;
        }
        body {
            background: #f0f2f5;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 20px;
        }
        .pos-container {
            max-width: 1400px;
            width: 100%;
            background: white;
            border-radius: 32px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.1);
            overflow: hidden;
            padding: 24px 28px;
        }
        h1 {
            font-size: 1.8rem;
            margin: 0 0 8px 0;
            color: #1e293b;
        }
        .sub {
            color: #475569;
            margin-bottom: 28px;
            border-left: 4px solid #3b82f6;
            padding-left: 12px;
        }
        .two-columns {
            display: flex;
            gap: 32px;
            flex-wrap: wrap;
        }
        .products-panel {
            flex: 1.2;
            background: #f8fafc;
            padding: 20px;
            border-radius: 24px;
            box-shadow: 0 1px 3px rgba(0,0,0,0.05);
        }
        .cart-panel {
            flex: 2;
        }
        .form-group {
            margin-bottom: 16px;
        }
        label {
            display: block;
            font-weight: 600;
            margin-bottom: 6px;
            color: #0f172a;
        }
        input {
            width: 100%;
            padding: 10px 14px;
            border: 1px solid #cbd5e1;
            border-radius: 16px;
            font-size: 1rem;
            transition: 0.2s;
        }
        input:focus {
            outline: none;
            border-color: #3b82f6;
            box-shadow: 0 0 0 3px rgba(59,130,246,0.2);
        }
        button {
            background: #3b82f6;
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 40px;
            font-weight: bold;
            cursor: pointer;
            font-size: 0.9rem;
            transition: 0.2s;
            margin-top: 8px;
        }
        button:hover {
            background: #2563eb;
            transform: scale(0.98);
        }
        .btn-danger {
            background: #ef4444;
        }
        .btn-danger:hover {
            background: #dc2626;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            background: white;
            border-radius: 20px;
            overflow: hidden;
            box-shadow: 0 1px 2px rgba(0,0,0,0.05);
        }
        th, td {
            text-align: left;
            padding: 12px 10px;
            border-bottom: 1px solid #e2e8f0;
            vertical-align: middle;
        }
        th {
            background-color: #f1f5f9;
            color: #1e293b;
        }
        .cart-totals {
            margin-top: 28px;
            background: #f1f5f9;
            padding: 18px 22px;
            border-radius: 24px;
            text-align: right;
        }
        .totals-line {
            display: flex;
            justify-content: space-between;
            font-size: 1.1rem;
            margin: 8px 0;
        }
        .grand-total {
            font-size: 1.6rem;
            font-weight: 800;
            color: #0f172a;
            border-top: 2px dashed #cbd5e1;
            padding-top: 12px;
            margin-top: 8px;
        }
        .empty-cart {
            text-align: center;
            padding: 40px;
            color: #64748b;
            font-style: italic;
        }
        .delete-btn {
            background: #fee2e2;
            color: #b91c1c;
            border: none;
            padding: 6px 12px;
            border-radius: 40px;
            cursor: pointer;
            font-weight: bold;
        }
        .delete-btn:hover {
            background: #fecaca;
        }
        .qty-container {
            display: flex;
            align-items: center;
            gap: 8px;
            flex-wrap: wrap;
        }
        .qty-btn {
            background: #e2e8f0;
            color: #1e293b;
            width: 28px;
            height: 28px;
            padding: 0;
            margin: 0;
            font-size: 1.2rem;
            font-weight: bold;
            display: inline-flex;
            align-items: center;
            justify-content: center;
            border-radius: 8px;
            cursor: pointer;
        }
        .qty-btn:hover {
            background: #cbd5e1;
            transform: none;
        }
        .qty-input {
            width: 60px;
            text-align: center;
            padding: 6px 4px;
            font-size: 0.9rem;
            border-radius: 12px;
        }
        
        /* Grid de productos con imágenes */
        .products-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(160px, 1fr));
            gap: 16px;
            margin-top: 20px;
            max-height: 500px;
            overflow-y: auto;
            padding: 8px 4px;
        }
        .product-card {
            background: white;
            border-radius: 20px;
            padding: 16px;
            text-align: center;
            cursor: pointer;
            transition: all 0.2s ease;
            border: 2px solid transparent;
            box-shadow: 0 2px 8px rgba(0,0,0,0.05);
        }
        .product-card:hover {
            transform: translateY(-4px);
            box-shadow: 0 8px 20px rgba(0,0,0,0.1);
            border-color: #3b82f6;
        }
        .product-card img {
            width: 80px;
            height: 80px;
            object-fit: contain;
            margin-bottom: 12px;
        }
        .product-card .product-name {
            font-weight: 700;
            color: #1e293b;
            margin: 8px 0 4px;
            font-size: 0.9rem;
        }
        .product-card .product-price {
            color: #3b82f6;
            font-weight: 600;
            font-size: 1.1rem;
        }
        .product-card .add-badge {
            margin-top: 10px;
            background: #3b82f6;
            color: white;
            border: none;
            padding: 6px 12px;
            border-radius: 40px;
            font-size: 0.75rem;
            font-weight: bold;
            cursor: pointer;
            width: 100%;
        }
        .product-card .add-badge:hover {
            background: #2563eb;
        }
        .quantity-control {
            margin-top: 12px;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
        }
        .quantity-control input {
            width: 60px;
            padding: 4px;
            text-align: center;
            margin: 0;
        }
        .section-title {
            font-size: 1.2rem;
            font-weight: 700;
            margin-bottom: 16px;
            color: #0f172a;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        @media (max-width: 800px) {
            .two-columns {
                flex-direction: column;
            }
            .products-grid {
                grid-template-columns: repeat(auto-fill, minmax(140px, 1fr));
            }
        }
    </style>
</head>
<body>
<div class="pos-container">
    <h1>🧾 Mini POS · Facturación con Imágenes</h1>
    <div class="sub">📌 Selecciona productos con imagen | Edita cantidades | Cálculo automático con IVA</div>

    <div class="two-columns">
        <!-- Panel izquierdo: productos con imágenes -->
        <div class="products-panel">
            <div class="section-title">
                🛍️ Productos disponibles
                <span style="font-size: 0.75rem; background: #e2e8f0; padding: 2px 8px; border-radius: 20px;">Haz clic en "Agregar"</span>
            </div>
            <div id="productsGrid" class="products-grid">
                <!-- Los productos se cargarán dinámicamente con JS -->
            </div>
        </div>

        <!-- Panel derecho: carrito y totales -->
        <div class="cart-panel">
            <div class="section-title">🛒 Carrito de compras</div>
            <div id="cartTableContainer">
                <table id="cartTable">
                    <thead>
                        <tr><th>Producto</th><th>Precio</th><th>Cantidad</th><th>Subtotal</th><th>Acciones</th></tr>
                    </thead>
                    <tbody id="cartBody">
                        <tr><td colspan="5" class="empty-cart">No hay productos agregados</td></tr>
                    </tbody>
                </table>
            </div>

            <div class="cart-totals">
                <div class="totals-line"><span>📊 Subtotal general:</span><span id="subtotalGeneral">0.00</span></div>
                <div class="totals-line"><span>🧾 IVA (19%):</span><span id="ivaAmount">0.00</span></div>
                <div class="totals-line grand-total"><span>💵 TOTAL A PAGAR:</span><span id="totalFinal">0.00</span></div>
            </div>
        </div>
    </div>
</div>

<script>
    // ==================== LISTA DE PRODUCTOS CON IMÁGENES (EMOJI COMO IMAGEN + TEXTO) ====================
    // Usamos emojis y FontAwesome para simular imágenes, pero puedes cambiarlo por URLs reales
    const productos = [
        { id: 1, nombre: "Smartphone X100", precio: 299.99, imagen: "📱", color: "#3b82f6" },
        { id: 2, nombre: "Laptop Pro 15", precio: 899.99, imagen: "💻", color: "#10b981" },
        { id: 3, nombre: "Audífonos Bluetooth", precio: 45.50, imagen: "🎧", color: "#8b5cf6" },
        { id: 4, nombre: "Smartwatch Sport", precio: 129.90, imagen: "⌚", color: "#ef4444" },
        { id: 5, nombre: "Mouse Inalámbrico", precio: 25.99, imagen: "🖱️", color: "#f59e0b" },
        { id: 6, nombre: "Teclado Mecánico", precio: 79.99, imagen: "⌨️", color: "#06b6d4" },
        { id: 7, nombre: "Cámara Web 4K", precio: 89.99, imagen: "📷", color: "#ec4899" },
        { id: 8, nombre: "Parlante Portátil", precio: 59.99, imagen: "🔊", color: "#14b8a6" },
        { id: 9, nombre: "SSD 1TB", precio: 119.99, imagen: "💾", color: "#6366f1" },
        { id: 10, nombre: "Cargador Rápido", precio: 19.99, imagen: "🔌", color: "#f97316" }
    ];
    
    // ---------- ARRAY QUE ALMACENA LOS PRODUCTOS DEL CARRITO ----------
    let cart = [];  // Cada elemento: { id, name, price, quantity, image }
    
    // Función auxiliar para generar IDs únicos para el carrito
    function generateCartId() {
        return Date.now() + '-' + Math.floor(Math.random() * 10000);
    }
    
    // Función para calcular subtotal de cada producto
    function calculateItemSubtotal(price, quantity) {
        return price * quantity;
    }
    
    // Función para recalcular TOTALES GLOBALES
    function updateTotals() {
        let subtotalGeneral = 0;
        for (let item of cart) {
            subtotalGeneral += item.price * item.quantity;
        }
        
        const IVA_RATE = 0.19;
        let iva = subtotalGeneral * IVA_RATE;
        let totalFinal = subtotalGeneral + iva;
        
        document.getElementById('subtotalGeneral').innerText = subtotalGeneral.toFixed(2);
        document.getElementById('ivaAmount').innerText = iva.toFixed(2);
        document.getElementById('totalFinal').innerText = totalFinal.toFixed(2);
    }
    
    // Función para modificar cantidad de un producto
    function updateQuantity(productId, newQuantity) {
        const product = cart.find(item => item.cartId === productId);
        if (product) {
            if (newQuantity < 1) {
                alert("La cantidad no puede ser menor a 1. Si deseas eliminar, usa el botón Eliminar.");
                return;
            }
            product.quantity = newQuantity;
            renderCart();
        }
    }
    
    // Función para eliminar producto del carrito
    function removeProductById(cartId) {
        cart = cart.filter(item => item.cartId !== cartId);
        renderCart();
    }
    
    // Función para RENDERIZAR el carrito con controles de cantidad
    function renderCart() {
        const tbody = document.getElementById('cartBody');
        tbody.innerHTML = '';
        
        if (cart.length === 0) {
            const emptyRow = document.createElement('tr');
            emptyRow.innerHTML = `<td colspan="5" class="empty-cart">🛍️ No hay productos agregados</td>`;
            tbody.appendChild(emptyRow);
        } else {
            for (let item of cart) {
                const subtotal = calculateItemSubtotal(item.price, item.quantity);
                const row = document.createElement('tr');
                
                // Nombre con imagen
                const tdName = document.createElement('td');
                tdName.innerHTML = `<span style="font-size: 1.5rem; margin-right: 8px;">${item.image}</span> ${item.name}`;
                
                // Precio unitario
                const tdPrice = document.createElement('td');
                tdPrice.textContent = `$${item.price.toFixed(2)}`;
                
                // Cantidad con controles
                const tdQty = document.createElement('td');
                const qtyContainer = document.createElement('div');
                qtyContainer.className = 'qty-container';
                
                const btnMinus = document.createElement('button');
                btnMinus.textContent = '−';
                btnMinus.className = 'qty-btn';
                btnMinus.addEventListener('click', () => updateQuantity(item.cartId, item.quantity - 1));
                
                const qtyInput = document.createElement('input');
                qtyInput.type = 'number';
                qtyInput.value = item.quantity;
                qtyInput.min = 1;
                qtyInput.className = 'qty-input';
                qtyInput.addEventListener('change', (e) => {
                    let newVal = parseInt(e.target.value);
                    if (isNaN(newVal)) newVal = 1;
                    updateQuantity(item.cartId, newVal);
                });
                
                const btnPlus = document.createElement('button');
                btnPlus.textContent = '+';
                btnPlus.className = 'qty-btn';
                btnPlus.addEventListener('click', () => updateQuantity(item.cartId, item.quantity + 1));
                
                qtyContainer.appendChild(btnMinus);
                qtyContainer.appendChild(qtyInput);
                qtyContainer.appendChild(btnPlus);
                tdQty.appendChild(qtyContainer);
                
                // Subtotal
                const tdSub = document.createElement('td');
                tdSub.textContent = `$${subtotal.toFixed(2)}`;
                tdSub.style.fontWeight = '500';
                
                // Botón eliminar
                const tdAction = document.createElement('td');
                const delButton = document.createElement('button');
                delButton.textContent = '🗑️ Eliminar';
                delButton.className = 'delete-btn';
                delButton.addEventListener('click', () => removeProductById(item.cartId));
                tdAction.appendChild(delButton);
                
                row.appendChild(tdName);
                row.appendChild(tdPrice);
                row.appendChild(tdQty);
                row.appendChild(tdSub);
                row.appendChild(tdAction);
                tbody.appendChild(row);
            }
        }
        updateTotals();
    }
    
    // Función para agregar producto al carrito
    function addProductToCart(product, quantity) {
        if (isNaN(quantity) || quantity < 1) {
            alert("❌ La cantidad debe ser al menos 1.");
            return false;
        }
        
        const newProduct = {
            cartId: generateCartId(),
            productId: product.id,
            name: product.nombre,
            price: product.precio,
            quantity: quantity,
            image: product.imagen
        };
        
        cart.push(newProduct);
        renderCart();
        return true;
    }
    
    // Renderizar grid de productos con imágenes
    function renderProductsGrid() {
        const grid = document.getElementById('productsGrid');
        grid.innerHTML = '';
        
        productos.forEach(producto => {
            const card = document.createElement('div');
            card.className = 'product-card';
            card.style.borderTop = `4px solid ${producto.color}`;
            
            // Imagen/Emoji grande
            const imgDiv = document.createElement('div');
            imgDiv.style.fontSize = '4rem';
            imgDiv.style.textAlign = 'center';
            imgDiv.textContent = producto.imagen;
            
            const nameDiv = document.createElement('div');
            nameDiv.className = 'product-name';
            nameDiv.textContent = producto.nombre;
            
            const priceDiv = document.createElement('div');
            priceDiv.className = 'product-price';
            priceDiv.textContent = `$${producto.precio.toFixed(2)}`;
            
            // Control de cantidad
            const qtyContainer = document.createElement('div');
            qtyContainer.className = 'quantity-control';
            const qtyInput = document.createElement('input');
            qtyInput.type = 'number';
            qtyInput.value = '1';
            qtyInput.min = '1';
            qtyInput.style.width = '60px';
            qtyInput.style.padding = '4px';
            qtyInput.style.borderRadius = '12px';
            qtyInput.style.border = '1px solid #cbd5e1';
            
            const addButton = document.createElement('button');
            addButton.textContent = '➕ Agregar';
            addButton.className = 'add-badge';
            addButton.style.margin = '0';
            addButton.style.padding = '6px 12px';
            addButton.addEventListener('click', (e) => {
                e.stopPropagation();
                const quantity = parseInt(qtyInput.value);
                addProductToCart(producto, quantity);
                qtyInput.value = '1'; // Resetear cantidad
            });
            
            qtyContainer.appendChild(qtyInput);
            qtyContainer.appendChild(addButton);
            
            card.appendChild(imgDiv);
            card.appendChild(nameDiv);
            card.appendChild(priceDiv);
            card.appendChild(qtyContainer);
            
            grid.appendChild(card);
        });
    }
    
    // ========== INICIALIZAR ==========
    function init() {
        renderProductsGrid();
        
        // Productos de ejemplo en el carrito
        cart = [
            { cartId: generateCartId(), productId: 1, name: "Smartphone X100", price: 299.99, quantity: 1, image: "📱" },
            { cartId: generateCartId(), productId: 3, name: "Audífonos Bluetooth", price: 45.50, quantity: 2, image: "🎧" },
            { cartId: generateCartId(), productId: 5, name: "Mouse Inalámbrico", price: 25.99, quantity: 1, image: "🖱️" }
        ];
        renderCart();
    }
    
    init();
</script>
</body>
</html>
