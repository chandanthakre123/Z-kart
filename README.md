// Z Kart - Complete Frontend with Checkout

import React, { useState } from "react";
import { BrowserRouter as Router, Routes, Route, Link } from "react-router-dom";

function Navbar() {
  return (
    <nav className="bg-white shadow-md p-4 flex justify-between items-center">
      <Link to="/" className="text-xl font-bold text-green-600">Z Kart</Link>
      <input
        type="text"
        placeholder="Search for products..."
        className="border p-2 rounded w-1/2"
      />
      <div className="space-x-4">
        <Link to="/products" className="text-gray-700 hover:text-green-600">Products</Link>
        <Link to="/cart" className="text-gray-700 hover:text-green-600">Cart</Link>
      </div>
    </nav>
  );
}

function Home() {
  const categories = ["Fruits & Vegetables", "Dairy", "Snacks", "Beverages"];
  return (
    <div className="p-6">
      <div className="text-center mb-8">
        <h1 className="text-3xl font-bold text-green-700">Welcome to Z Kart</h1>
        <p className="text-gray-600">Fast delivery of your daily needs!</p>
      </div>
      <div className="grid grid-cols-2 md:grid-cols-4 gap-4">
        {categories.map((cat, idx) => (
          <div
            key={idx}
            className="bg-white p-4 rounded shadow hover:shadow-lg text-center cursor-pointer"
          >
            <h2 className="text-lg font-semibold text-gray-700">{cat}</h2>
          </div>
        ))}
      </div>
    </div>
  );
}

function Products() {
  const products = [
    { name: "Apple", price: 120, image: "https://via.placeholder.com/150" },
    { name: "Milk", price: 45, image: "https://via.placeholder.com/150" },
    { name: "Chips", price: 30, image: "https://via.placeholder.com/150" },
    { name: "Juice", price: 60, image: "https://via.placeholder.com/150" },
  ];

  return (
    <div className="p-6">
      <h2 className="text-2xl font-bold mb-4 text-green-700">All Products</h2>
      <div className="grid grid-cols-2 md:grid-cols-4 gap-4">
        {products.map((item, idx) => (
          <div
            key={idx}
            className="bg-white p-4 rounded shadow hover:shadow-lg text-center"
          >
            <img src={item.image} alt={item.name} className="w-full h-32 object-cover mb-2 rounded" />
            <h3 className="text-lg font-semibold text-gray-700">{item.name}</h3>
            <p className="text-green-600 font-bold">₹{item.price}</p>
            <button className="mt-2 px-4 py-2 bg-green-600 text-white rounded hover:bg-green-700">
              Add to Cart
            </button>
          </div>
        ))}
      </div>
    </div>
  );
}

function Cart() {
  const [cartItems, setCartItems] = useState([
    { name: "Apple", price: 120, quantity: 1 },
    { name: "Milk", price: 45, quantity: 2 },
  ]);

  const updateQuantity = (index, delta) => {
    const updated = [...cartItems];
    updated[index].quantity += delta;
    if (updated[index].quantity < 1) updated[index].quantity = 1;
    setCartItems(updated);
  };

  const total = cartItems.reduce((sum, item) => sum + item.price * item.quantity, 0);

  return (
    <div className="p-6">
      <h2 className="text-2xl font-bold mb-4 text-green-700">Your Cart</h2>
      <div className="space-y-4">
        {cartItems.map((item, idx) => (
          <div key={idx} className="bg-white p-4 rounded shadow flex justify-between items-center">
            <div>
              <h3 className="text-lg font-semibold text-gray-700">{item.name}</h3>
              <p className="text-gray-600">₹{item.price} x {item.quantity}</p>
            </div>
            <div className="flex items-center space-x-2">
              <button onClick={() => updateQuantity(idx, -1)} className="px-2 py-1 bg-gray-200 rounded">-</button>
              <span>{item.quantity}</span>
              <button onClick={() => updateQuantity(idx, 1)} className="px-2 py-1 bg-gray-200 rounded">+</button>
            </div>
          </div>
        ))}
      </div>
      <div className="mt-6 text-right">
        <p className="text-xl font-bold text-green-700">Total: ₹{total}</p>
        <Link to="/checkout">
          <button className="mt-4 px-6 py-2 bg-green-600 text-white rounded hover:bg-green-700">
            Proceed to Checkout
          </button>
        </Link>
      </div>
    </div>
  );
}

function Checkout() {
  const [deliveryTime, setDeliveryTime] = useState("");

  return (
    <div className="p-6">
      <h2 className="text-2xl font-bold mb-4 text-green-700">Checkout</h2>
      <div className="bg-white p-4 rounded shadow mb-4">
        <label className="block text-gray-700 mb-2">Choose Delivery Time:</label>
        <select
          value={deliveryTime}
          onChange={(e) => setDeliveryTime(e.target.value)}
          className="border rounded p-2 w-full"
        >
          <option value="">Select a time slot</option>
          <option>8 AM - 10 AM</option>
          <option>10 AM - 12 PM</option>
          <option>12 PM - 2 PM</option>
          <option>4 PM - 6 PM</option>
        </select>
      </div>

      <div className="bg-white p-4 rounded shadow">
        <h3 className="text-lg font-semibold text-gray-700 mb-2">Payment</h3>
        <p className="text-gray-500">(Payment gateway integration coming soon)</p>
        <button className="mt-4 px-6 py-2 bg-green-600 text-white rounded hover:bg-green-700">
          Place Order
        </button>
      </div>
    </div>
  );
}

export default function App() {
  return (
    <Router>
      <div className="min-h-screen bg-gray-50">
        <Navbar />
        <Routes>
          <Route path="/" element={<Home />} />
          <Route path="/products" element={<Products />} />
          <Route path="/cart" element={<Cart />} />
          <Route path="/checkout" element={<Checkout />} />
        </Routes>
      </div>
    </Router>
  );
}
