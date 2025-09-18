// App.jsx
import React, { useState, useEffect } from 'react';
import { Search, ShoppingCart, User, Heart, Menu, X, Star, Filter, ChevronDown, Plus, Minus } from 'lucide-react';
import { motion } from 'framer-motion';

// Mock data
const mockProducts = [
  {
    id: 1,
    name: "Heritage Leather Handbag",
    price: 299,
    originalPrice: 399,
    image: "https://placehold.co/400x400/D4AF37/FFFFFF?text=Heritage+Bag",
    category: "Handbags",
    rating: 4.8,
    reviews: 124,
    inStock: true
  },
  {
    id: 2,
    name: "Timeless Gold Watch",
    price: 599,
    originalPrice: 799,
    image: "https://placehold.co/400x400/1C1C1C/FFFFFF?text=Gold+Watch",
    category: "Watches",
    rating: 4.9,
    reviews: 89,
    inStock: true
  },
  {
    id: 3,
    name: "Minimalist Silk Scarf",
    price: 149,
    originalPrice: 199,
    image: "https://placehold.co/400x400/D4AF37/1C1C1C?text=Silk+Scarf",
    category: "Accessories",
    rating: 4.7,
    reviews: 67,
    inStock: false
  },
  {
    id: 4,
    name: "Classic Leather Wallet",
    price: 199,
    originalPrice: 249,
    image: "https://placehold.co/400x400/1C1C1C/D4AF37?text=Leather+Wallet",
    category: "Accessories",
    rating: 4.6,
    reviews: 156,
    inStock: true
  }
];

const categories = ["All", "Handbags", "Watches", "Accessories", "Jewelry"];

export default function App() {
  const [isMenuOpen, setIsMenuOpen] = useState(false);
  const [cartItems, setCartItems] = useState([]);
  const [wishlistItems, setWishlistItems] = useState([]);
  const [selectedCategory, setSelectedCategory] = useState("All");
  const [searchQuery, setSearchQuery] = useState("");
  const [isCartOpen, setIsCartOpen] = useState(false);
  const [isWishlistOpen, setIsWishlistOpen] = useState(false);

  // Filter products based on category and search
  const filteredProducts = mockProducts.filter(product => {
    const matchesCategory = selectedCategory === "All" || product.category === selectedCategory;
    const matchesSearch = product.name.toLowerCase().includes(searchQuery.toLowerCase());
    return matchesCategory && matchesSearch;
  });

  // Add to cart
  const addToCart = (product) => {
    setCartItems(prev => {
      const existingItem = prev.find(item => item.id === product.id);
      if (existingItem) {
        return prev.map(item => 
          item.id === product.id 
            ? { ...item, quantity: item.quantity + 1 }
            : item
        );
      }
      return [...prev, { ...product, quantity: 1 }];
    });
  };

  // Remove from cart
  const removeFromCart = (productId) => {
    setCartItems(prev => prev.filter(item => item.id !== productId));
  };

  // Update quantity
  const updateQuantity = (productId, newQuantity) => {
    if (newQuantity === 0) {
      removeFromCart(productId);
      return;
    }
    setCartItems(prev => 
      prev.map(item => 
        item.id === productId 
          ? { ...item, quantity: newQuantity }
          : item
      )
    );
  };

  // Add to wishlist
  const addToWishlist = (product) => {
    if (!wishlistItems.find(item => item.id === product.id)) {
      setWishlistItems(prev => [...prev, product]);
    }
  };

  // Remove from wishlist
  const removeFromWishlist = (productId) => {
    setWishlistItems(prev => prev.filter(item => item.id !== productId));
  };

  // Cart total
  const cartTotal = cartItems.reduce((total, item) => total + (item.price * item.quantity), 0);

  return (
    <div className="min-h-screen bg-white">
      {/* Header */}
      <header className="sticky top-0 z-50 bg-white border-b border-gray-100">
        <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
          <div className="flex items-center justify-between h-16">
            {/* Logo */}
            <div className="flex items-center">
              <div className="flex-shrink-0 flex items-center">
                <div className="h-8 w-8 bg-[#D4AF37] rounded-full flex items-center justify-center">
                  <span className="text-white text-sm font-bold">∞</span>
                </div>
                <span className="ml-2 text-xl font-serif font-bold text-[#1C1C1C]">RIVELLE</span>
              </div>
            </div>

            {/* Desktop Navigation */}
            <nav className="hidden md:flex space-x-8">
              {categories.map(category => (
                <button
                  key={category}
                  onClick={() => setSelectedCategory(category)}
                  className={`text-sm font-medium transition-colors ${
                    selectedCategory === category 
                      ? 'text-[#D4AF37]' 
                      : 'text-gray-700 hover:text-[#D4AF37]'
                  }`}
                >
                  {category}
                </button>
              ))}
            </nav>

            {/* Search Bar */}
            <div className="hidden md:flex flex-1 max-w-lg mx-8">
              <div className="relative w-full">
                <Search className="absolute left-3 top-1/2 transform -translate-y-1/2 text-gray-400 h-4 w-4" />
                <input
                  type="text"
                  placeholder="Search products..."
                  value={searchQuery}
                  onChange={(e) => setSearchQuery(e.target.value)}
                  className="w-full pl-10 pr-4 py-2 border border-gray-200 rounded-lg focus:outline-none focus:ring-2 focus:ring-[#D4AF37] focus:border-transparent"
                />
              </div>
            </div>

            {/* Right Icons */}
            <div className="flex items-center space-x-4">
              <button 
                onClick={() => setIsWishlistOpen(true)}
                className="relative p-2 text-gray-700 hover:text-[#D4AF37] transition-colors"
              >
                <Heart className="h-5 w-5" />
                {wishlistItems.length > 0 && (
                  <span className="absolute -top-1 -right-1 bg-[#D4AF37] text-white text-xs rounded-full h-5 w-5 flex items-center justify-center">
                    {wishlistItems.length}
                  </span>
                )}
              </button>
              
              <button 
                onClick={() => setIsCartOpen(true)}
                className="relative p-2 text-gray-700 hover:text-[#D4AF37] transition-colors"
              >
                <ShoppingCart className="h-5 w-5" />
                {cartItems.length > 0 && (
                  <span className="absolute -top-1 -right-1 bg-[#D4AF37] text-white text-xs rounded-full h-5 w-5 flex items-center justify-center">
                    {cartItems.reduce((sum, item) => sum + item.quantity, 0)}
                  </span>
                )}
              </button>
              
              <button className="p-2 text-gray-700 hover:text-[#D4AF37] transition-colors">
                <User className="h-5 w-5" />
              </button>
              
              <button 
                onClick={() => setIsMenuOpen(!isMenuOpen)}
                className="md:hidden p-2 text-gray-700"
              >
                {isMenuOpen ? <X className="h-5 w-5" /> : <Menu className="h-5 w-5" />}
              </button>
            </div>
          </div>

          {/* Mobile Search */}
          <div className="md:hidden pb-4">
            <div className="relative">
              <Search className="absolute left-3 top-1/2 transform -translate-y-1/2 text-gray-400 h-4 w-4" />
              <input
                type="text"
                placeholder="Search products..."
                value={searchQuery}
                onChange={(e) => setSearchQuery(e.target.value)}
                className="w-full pl-10 pr-4 py-2 border border-gray-200 rounded-lg focus:outline-none focus:ring-2 focus:ring-[#D4AF37] focus:border-transparent"
              />
            </div>
          </div>
        </div>

        {/* Mobile Menu */}
        {isMenuOpen && (
          <motion.div 
            initial={{ opacity: 0, height: 0 }}
            animate={{ opacity: 1, height: 'auto' }}
            exit={{ opacity: 0, height: 0 }}
            className="md:hidden border-t border-gray-100"
          >
            <div className="px-4 py-2 space-y-1">
              {categories.map(category => (
                <button
                  key={category}
                  onClick={() => {
                    setSelectedCategory(category);
                    setIsMenuOpen(false);
                  }}
                  className={`block w-full text-left px-3 py-2 text-base font-medium transition-colors ${
                    selectedCategory === category 
                      ? 'text-[#D4AF37] bg-gray-50' 
                      : 'text-gray-700 hover:text-[#D4AF37] hover:bg-gray-50'
                  }`}
                >
                  {category}
                </button>
              ))}
            </div>
          </motion.div>
        )}
      </header>

      {/* Hero Section */}
      <section className="relative bg-gradient-to-r from-[#1C1C1C] to-gray-900 text-white">
        <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-24">
          <div className="text-center">
            <motion.h1 
              initial={{ opacity: 0, y: 20 }}
              animate={{ opacity: 1, y: 0 }}
              transition={{ duration: 0.8 }}
              className="text-4xl md:text-6xl font-serif font-bold mb-6"
            >
              Timeless Luxury
            </motion.h1>
            <motion.p 
              initial={{ opacity: 0, y: 20 }}
              animate={{ opacity: 1, y: 0 }}
              transition={{ duration: 0.8, delay: 0.2 }}
              className="text-xl md:text-2xl mb-8 text-gray-300"
            >
              Discover our curated collection of heritage-inspired pieces
            </motion.p>
            <motion.button 
              initial={{ opacity: 0, y: 20 }}
              animate={{ opacity: 1, y: 0 }}
              transition={{ duration: 0.8, delay: 0.4 }}
              className="bg-[#D4AF37] text-[#1C1C1C] px-8 py-3 rounded-lg font-semibold text-lg hover:bg-[#c4a029] transition-colors"
            >
              Shop Collection
            </motion.button>
          </div>
        </div>
      </section>

      {/* Products Section */}
      <section className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-16">
        <div className="flex flex-col md:flex-row md:items-center md:justify-between mb-8">
          <h2 className="text-3xl font-serif font-bold text-[#1C1C1C] mb-4 md:mb-0">
            Featured Products
          </h2>
          <div className="flex items-center space-x-4">
            <Filter className="h-5 w-5 text-gray-500" />
            <select className="border border-gray-200 rounded-lg px-3 py-2 focus:outline-none focus:ring-2 focus:ring-[#D4AF37]">
              <option>Sort by: Featured</option>
              <option>Price: Low to High</option>
              <option>Price: High to Low</option>
              <option>Customer Rating</option>
            </select>
          </div>
        </div>

        <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-8">
          {filteredProducts.map((product) => (
            <motion.div
              key={product.id}
              initial={{ opacity: 0, y: 20 }}
              animate={{ opacity: 1, y: 0 }}
              transition={{ duration: 0.5 }}
              className="group relative bg-white rounded-lg shadow-sm hover:shadow-lg transition-shadow border border-gray-100"
            >
              <div className="aspect-w-1 aspect-h-1 w-full overflow-hidden rounded-t-lg">
                <img
                  src={product.image}
                  alt={product.name}
                  className="w-full h-64 object-cover group-hover:scale-105 transition-transform duration-300"
                />
                {!product.inStock && (
                  <div className="absolute inset-0 bg-black bg-opacity-50 flex items-center justify-center">
                    <span className="text-white font-semibold">Out of Stock</span>
                  </div>
                )}
              </div>
              
              <div className="p-4">
                <div className="flex items-center mb-2">
                  <div className="flex items-center">
                    {[...Array(5)].map((_, i) => (
                      <Star
                        key={i}
                        className={`h-4 w-4 ${
                          i < Math.floor(product.rating)
                            ? 'text-yellow-400 fill-current'
                            : 'text-gray-300'
                        }`}
                      />
                    ))}
                  </div>
                  <span className="ml-2 text-sm text-gray-500">
                    {product.rating} ({product.reviews})
                  </span>
                </div>
                
                <h3 className="text-lg font-semibold text-[#1C1C1C] mb-2">
                  {product.name}
                </h3>
                
                <div className="flex items-center justify-between">
                  <div className="flex items-center space-x-2">
                    <span className="text-lg font-bold text-[#1C1C1C]">
                      ${product.price}
                    </span>
                    {product.originalPrice > product.price && (
                      <span className="text-sm text-gray-500 line-through">
                        ${product.originalPrice}
                      </span>
                    )}
                  </div>
                </div>
                
                <div className="mt-4 flex space-x-2">
                  <button
                    onClick={() => addToCart(product)}
                    disabled={!product.inStock}
                    className={`flex-1 py-2 px-4 rounded-lg font-medium transition-colors ${
                      product.inStock
                        ? 'bg-[#1C1C1C] text-white hover:bg-gray-800'
                        : 'bg-gray-100 text-gray-400 cursor-not-allowed'
                    }`}
                  >
                    {product.inStock ? 'Add to Cart' : 'Out of Stock'}
                  </button>
                  <button
                    onClick={() => addToWishlist(product)}
                    className="p-2 border border-gray-200 rounded-lg hover:border-[#D4AF37] transition-colors"
                  >
                    <Heart className="h-4 w-4" />
                  </button>
                </div>
              </div>
            </motion.div>
          ))}
        </div>
      </section>

      {/* Cart Sidebar */}
      {isCartOpen && (
        <div className="fixed inset-0 z-50 overflow-hidden">
          <div className="absolute inset-0 bg-black bg-opacity-50" onClick={() => setIsCartOpen(false)} />
          <div className="absolute right-0 top-0 h-full w-full max-w-md bg-white shadow-xl">
            <div className="flex flex-col h-full">
              <div className="flex items-center justify-between p-4 border-b">
                <h2 className="text-lg font-semibold">Shopping Cart</h2>
                <button onClick={() => setIsCartOpen(false)} className="p-2">
                  <X className="h-5 w-5" />
                </button>
              </div>
              
              <div className="flex-1 overflow-y-auto p-4">
                {cartItems.length === 0 ? (
                  <div className="text-center py-8">
                    <ShoppingCart className="h-12 w-12 text-gray-300 mx-auto mb-4" />
                    <p className="text-gray-500">Your cart is empty</p>
                  </div>
                ) : (
                  <div className="space-y-4">
                    {cartItems.map((item) => (
                      <div key={item.id} className="flex items-center space-x-4 p-4 border rounded-lg">
                        <img
                          src={item.image}
                          alt={item.name}
                          className="w-16 h-16 object-cover rounded"
                        />
                        <div className="flex-1">
                          <h3 className="font-medium text-sm">{item.name}</h3>
                          <p className="text-[#D4AF37] font-semibold">${item.price}</p>
                          <div className="flex items-center space-x-2 mt-2">
                            <button
                              onClick={() => updateQuantity(item.id, item.quantity - 1)}
                              className="p-1 rounded-full border"
                            >
                              <Minus className="h-3 w-3" />
                            </button>
                            <span className="text-sm">{item.quantity}</span>
                            <button
                              onClick={() => updateQuantity(item.id, item.quantity + 1)}
                              className="p-1 rounded-full border"
                            >
                              <Plus className="h-3 w-3" />
                            </button>
                          </div>
                        </div>
                        <button
                          onClick={() => removeFromCart(item.id)}
                          className="p-2 text-gray-400 hover:text-red-500"
                        >
                          <X className="h-4 w-4" />
                        </button>
                      </div>
                    ))}
                  </div>
                )}
              </div>
              
              {cartItems.length > 0 && (
                <div className="border-t p-4">
                  <div className="flex justify-between items-center mb-4">
                    <span className="font-semibold">Total:</span>
                    <span className="text-xl font-bold text-[#D4AF37]">${cartTotal.toFixed(2)}</span>
                  </div>
                  <button className="w-full bg-[#D4AF37] text-[#1C1C1C] py-3 rounded-lg font-semibold hover:bg-[#c4a029] transition-colors">
                    Proceed to Checkout
                  </button>
                </div>
              )}
            </div>
          </div>
        </div>
      )}

      {/* Wishlist Sidebar */}
      {isWishlistOpen && (
        <div className="fixed inset-0 z-50 overflow-hidden">
          <div className="absolute inset-0 bg-black bg-opacity-50" onClick={() => setIsWishlistOpen(false)} />
          <div className="absolute right-0 top-0 h-full w-full max-w-md bg-white shadow-xl">
            <div className="flex flex-col h-full">
              <div className="flex items-center justify-between p-4 border-b">
                <h2 className="text-lg font-semibold">Wishlist</h2>
                <button onClick={() => setIsWishlistOpen(false)} className="p-2">
                  <X className="h-5 w-5" />
                </button>
              </div>
              
              <div className="flex-1 overflow-y-auto p-4">
                {wishlistItems.length === 0 ? (
                  <div className="text-center py-8">
                    <Heart className="h-12 w-12 text-gray-300 mx-auto mb-4" />
                    <p className="text-gray-500">Your wishlist is empty</p>
                  </div>
                ) : (
                  <div className="space-y-4">
                    {wishlistItems.map((item) => (
                      <div key={item.id} className="flex items-center space-x-4 p-4 border rounded-lg">
                        <img
                          src={item.image}
                          alt={item.name}
                          className="w-16 h-16 object-cover rounded"
                        />
                        <div className="flex-1">
                          <h3 className="font-medium text-sm">{item.name}</h3>
                          <p className="text-[#D4AF37] font-semibold">${item.price}</p>
                        </div>
                        <div className="flex flex-col space-y-2">
                          <button
                            onClick={() => addToCart(item)}
                            className="p-2 bg-[#1C1C1C] text-white rounded text-xs"
                          >
                            Add to Cart
                          </button>
                          <button
                            onClick={() => removeFromWishlist(item.id)}
                            className="p-2 text-gray-400 hover:text-red-500"
                          >
                            <X className="h-4 w-4" />
                          </button>
                        </div>
                      </div>
                    ))}
                  </div>
                )}
              </div>
            </div>
          </div>
        </div>
      )}

      {/* Footer */}
      <footer className="bg-[#1C1C1C] text-white">
        <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-12">
          <div className="grid grid-cols-1 md:grid-cols-4 gap-8">
            <div>
              <div className="flex items-center mb-4">
                <div className="h-8 w-8 bg-[#D4AF37] rounded-full flex items-center justify-center">
                  <span className="text-white text-sm font-bold">∞</span>
                </div>
                <span className="ml-2 text-xl font-serif font-bold">RIVELLE</span>
              </div>
              <p className="text-gray-400 text-sm">
                Timeless luxury crafted with heritage and inspired by the future.
              </p>
            </div>
            
            <div>
              <h3 className="font-semibold mb-4">Shop</h3>
              <ul className="space-y-2 text-sm text-gray-400">
                <li><a href="#" className="hover:text-[#D4AF37] transition-colors">New Arrivals</a></li>
                <li><a href="#" className="hover:text-[#D4AF37] transition-colors">Best Sellers</a></li>
                <li><a href="#" className="hover:text-[#D4AF37] transition-colors">Sale</a></li>
                <li><a href="#" className="hover:text-[#D4AF37] transition-colors">Collections</a></li>
              </ul>
            </div>
            
            <div>
              <h3 className="font-semibold mb-4">Support</h3>
              <ul className="space-y-2 text-sm text-gray-400">
                <li><a href="#" className="hover:text-[#D4AF37] transition-colors">Contact Us</a></li>
                <li><a href="#" className="hover:text-[#D4AF37] transition-colors">Shipping Info</a></li>
                <li><a href="#" className="hover:text-[#D4AF37] transition-colors">Returns</a></li>
                <li><a href="#" className="hover:text-[#D4AF37] transition-colors">FAQ</a></li>
              </ul>
            </div>
            
            <div>
              <h3 className="font-semibold mb-4">Connect</h3>
              <ul className="space-y-2 text-sm text-gray-400">
                <li><a href="#" className="hover:text-[#D4AF37] transition-colors">Instagram</a></li>
                <li><a href="#" className="hover:text-[#D4AF37] transition-colors">Facebook</a></li>
                <li><a href="#" className="hover:text-[#D4AF37] transition-colors">Twitter</a></li>
                <li><a href="#" className="hover:text-[#D4AF37] transition-colors">Pinterest</a></li>
              </ul>
            </div>
          </div>
          
          <div className="border-t border-gray-800 mt-8 pt-8 text-center text-sm text-gray-400">
            <p>&copy; 2024 Rivelle. All rights reserved.</p>
          </div>
        </div>
      </footer>
    </div>
  );
}
