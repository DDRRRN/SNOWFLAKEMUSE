<html lang="en">
<head>
    <meta charset="utf-8"/>
    <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
    <title>SnowflakeMuse</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.3/css/all.min.css" rel="stylesheet"/>
    <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet"/>
    <style>
        body {
            font-family: 'Roboto', sans-serif;
            background: linear-gradient(135deg, #1f1c2c, #928DAB); /* Gradient background */
            color: #f5f5f5; /* Light text color for better contrast */
        }
        .hidden { display: none; }
    </style>
</head>
<body>
    <header class="bg-gray-800 p-4 flex justify-between items-center">
        <h1 class="text-3xl font-bold">SnowflakeMuse</h1>
    </header>
    <main class="container mx-auto p-4">
        <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-6" id="product-list"></div>
    </main>
    <footer class="bg-gray-800 p-4 mt-8 text-center">
        <p>© 2023 SnowflakeMuse. All rights reserved.</p>
    </footer>

    <!-- Contact Modal -->
    <div id="contactModal" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center hidden" role="dialog" aria-labelledby="modalTitle" aria-describedby="modalDescription">
        <div class="bg-gray-800 p-6 rounded-lg shadow-lg text-center relative w-96 border-2 border-gray-600">
            <h2 class="text-2xl font-bold mb-4" id="modalTitle">Invoice</h2>
            <div class="absolute inset-0 opacity-10 flex justify-center items-center text-6xl font-bold">DAREKANOAISURU</div>
            <div class="mb-4 relative text-gray-300" id="invoiceDetails"></div>
            <div class="text-lg font-semibold text-gray-200 mb-4">
                <div class="flex justify-between">
                    <span>Total Harga:</span>
                    <span id="totalPrice"></span>
                </div>
            </div>
            <p class="text-sm text-gray-400 relative">Silakan screenshot dan hubungi kontak di bawah.</p>
            <a class="bg-green-500 text-white px-4 py-2 rounded-lg hover:bg-green-600 mt-2 block" href="https://wa.me/+6285264149438" target="_blank" rel="noopener noreferrer">WhatsApp</a>
            <p class="mt-2 relative">Discord: darekanoaisuru</p>
            <button class="absolute top-2 right-2 text-gray-400 hover:text-gray-200 text-xl" id="closeModal" aria-label="Close modal">✖</button>
        </div>
    </div>

    <script>
        // Only include the specified products
        const products = [
            { name: "Rank Spin", description: "1x spin = 10k\n5x spin = 15k\n10x spin = 18k", price: 10000 },
            { name: "Pet Ranks", description: "Gives pet rank for better defense", price: 15000 }
        ];

        const productList = document.getElementById('product-list');

        function renderProducts() {
            productList.innerHTML = ""; // Clear existing products
            products.forEach((product, index) => {
                const productCard = document.createElement('div');
                productCard.classList.add('bg-gray-800', 'p-4', 'rounded-lg', 'shadow-lg');
                
                productCard.innerHTML = `
                    <h2 class="text-xl font-bold mb-2">${product.name}</h2>
                    <p class="w-full p-2 mb-2 bg-gray-700 text-gray-300 rounded-lg">${product.description.replace(/\n/g, '<br>')}</p>
                    <p class="text-lg font-semibold mb-4">IDR ${product.price.toLocaleString()}</p>
                    <label class="block mb-2" for="quantity-${index}">Jumlah:</label>
                    <input type="number" id="quantity-${index}" class="w-full p-2 mb-2 bg-gray-700 text-black rounded-lg" min="1" value="1" required/>
                    <label class="block mb-2" for="notes-${index}">Catatan:</label>
                    <textarea id="notes-${index}" class="w-full p-2 mb-2 bg-gray-700 text-black rounded-lg" placeholder="Optional notes..."></textarea>
                    <button class="bg-green-500 text-white px-4 py-2 rounded-lg hover:bg-green-600 buy-btn" data-index="${index}">Beli</button>
                `;
                
                productList.appendChild(productCard);
            });
            attachEventListeners();
        }
        
        function attachEventListeners() {
            document.querySelectorAll('.buy-btn').forEach(button => {
                button.addEventListener('click', (e) => {
                    const index = e.target.dataset.index;
                    const quantity = document.getElementById(`quantity-${index}`).value;
                    const notes = document.getElementById(`notes-${index}`).value;

                    // Validate quantity
                    if (quantity < 1) {
                        alert("Jumlah harus lebih dari 0.");
                        return;
                    }

                    const product = products[index];
                    const totalPrice = product.price * quantity;
                    const invoiceDetails = `
                        <div class="flex justify-between mb-2">
                            <span><strong>Produk:</strong></span>
                            <span>${product.name}</span>
                        </div>
                        <div class="flex justify-between mb-2">
                            <span><strong>Deskripsi:</strong></span>
                            <span>${product.description.replace(/\n/g, '<br>')}</span>
                        </div>
                        <div class="flex justify-between mb-2">
                            <span><strong>Harga:</strong></span>
                            <span>IDR ${product.price.toLocaleString()}</span>
                        </div>
                        <div class="flex justify-between mb-2">
                            <span><strong>Jumlah:</strong></span>
                            <span>${quantity}</span>
                        </div>
                        <div class="flex justify-between mb-2">
                            <span><strong>Catatan:</strong></span>
                            <span>${notes || 'Tidak ada catatan'}</span>
                        </div>
                    `;
                    
                    document.getElementById('invoiceDetails').innerHTML = invoiceDetails;
                    document.getElementById('totalPrice').innerText = `IDR ${totalPrice.toLocaleString()}`;
                    document.getElementById('contactModal').classList.remove('hidden');
                });
            });
        }
        
        document.getElementById('closeModal').addEventListener('click', () => {
            document.getElementById('contactModal').classList.add('hidden');
        });

        // Call renderProducts only once when the page loads
        window.onload = renderProducts;
    </script>
</body>
</html>
