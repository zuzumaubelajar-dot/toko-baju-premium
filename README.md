"use client";
import React, { useState } from 'react';
import { supabase } from '@/lib/supabase';
import { PlusCircle, Loader2 } from 'lucide-react';

export default function AdminPage() {
  const [loading, setLoading] = useState(false);
  const [formData, setFormData] = useState({
    name: '', price: '', category: '', image: '', description: ''
  });

  const handleSubmit = async (e: React.FormEvent) => {
    e.preventDefault();
    setLoading(true);

    const { error } = await supabase.from('products').insert([
      { ...formData, price: parseInt(formData.price), rating: 5 }
    ]);

    setLoading(false);
    if (error) alert("Gagal tambah produk: " + error.message);
    else {
      alert("Produk berhasil ditambahkan!");
      setFormData({ name: '', price: '', category: '', image: '', description: '' });
    }
  };

  return (
    <div className="min-h-screen bg-gray-100 p-8">
      <div className="max-w-2xl mx-auto bg-white rounded-2xl shadow-xl p-8">
        <h1 className="text-2xl font-bold mb-6 flex items-center gap-2">
          <PlusCircle className="text-indigo-600" /> Dashboard Admin Toko
        </h1>
        
        <form onSubmit={handleSubmit} className="space-y-4">
          <div>
            <label className="block text-sm font-medium mb-1">Nama Produk</label>
            <input type="text" required className="w-full p-3 border rounded-lg" 
              value={formData.name} onChange={(e) => setFormData({...formData, name: e.target.value})} />
          </div>
          <div className="grid grid-cols-2 gap-4">
            <div>
              <label className="block text-sm font-medium mb-1">Harga (IDR)</label>
              <input type="number" required className="w-full p-3 border rounded-lg" 
                value={formData.price} onChange={(e) => setFormData({...formData, price: e.target.value})} />
            </div>
            <div>
              <label className="block text-sm font-medium mb-1">Kategori</label>
              <input type="text" required className="w-full p-3 border rounded-lg" 
                value={formData.category} onChange={(e) => setFormData({...formData, category: e.target.value})} />
            </div>
          </div>
          <div>
            <label className="block text-sm font-medium mb-1">URL Gambar (Unsplash/Link)</label>
            <input type="text" required className="w-full p-3 border rounded-lg" 
              value={formData.image} onChange={(e) => setFormData({...formData, image: e.target.value})} />
          </div>
          <div>
            <label className="block text-sm font-medium mb-1">Deskripsi</label>
            <textarea required className="w-full p-3 border rounded-lg h-32" 
              value={formData.description} onChange={(e) => setFormData({...formData, description: e.target.value})} />
          </div>
          
          <button type="submit" disabled={loading}
            className="w-full bg-indigo-600 text-white py-4 rounded-xl font-bold hover:bg-indigo-700 disabled:bg-gray-400">
            {loading ? <Loader2 className="animate-spin mx-auto" /> : "Simpan Produk Baru"}
          </button>
        </form>
      </div>
    </div>
  );
}
