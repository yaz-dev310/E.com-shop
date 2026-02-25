<script setup>
import { ref, computed } from 'vue';

// Données des produits avec les nouveaux prix en FCFA
const products = ref([
  { id: 1, name: 'KISS Alive Worldwide 96/97', category: 'VINTAGE', price: '25000', rating: '5.0 (89)', image: '/images/IMG_3018.png', tag: 'Rare' },
  { id: 2, name: 'Polo Sports Division', category: 'STREETWEAR', price: '20000', rating: '4.9 (120)', image: '/images/IMG_3008.png', tag: 'Nouveau' },
  { id: 3, name: 'Valley Dreams Golden Nights', category: 'VINTAGE', price: '25000', rating: '4.7 (75)', image: '/images/IMG_3045.png', tag: 'Top Ventes' },
  { id: 4, name: 'Chandelier Interior Sketch', category: 'ART', price: '20000', rating: '4.9 (34)', image: '/images/IMG_2868.png', tag: 'Édition Limitée' },
  { id: 5, name: 'A Trial Ends Text Tee', category: 'MINIMAL', price: '20000', rating: '4.6 (52)', image: '/images/IMG_2878.png', tag: 'Premium' },
  { id: 6, name: 'Roughplay NYPD Edition', category: 'STREETWEAR', price: '25000', rating: '5.0 (108)', image: '/images/IMG_2889.png', tag: 'Must Have' },
  { id: 7, name: 'Human Target Graphic', category: 'GRAPHIC', price: '20000', rating: '4.8 (91)', image: '/images/IMG_2956.png', tag: 'Nouveau' },
  { id: 8, name: 'West Coast Choppers Custom', category: 'BIKER', price: '25000', rating: '4.7 (64)', image: '/images/IMG_2991.png', tag: 'Vintage' },
  { id: 9, name: 'Lost Intricacy Skeleton', category: 'DARK', price: '25000', rating: '4.9 (82)', image: '/images/IMG_3055.png', tag: 'Rare' },
  { id: 10, name: 'Cupid Archery Stone Wash', category: 'REPRESENT', price: '25000', rating: '4.8 (115)', image: '/images/IMG_3065 (1).png', tag: 'Nouveau' },
  { id: 11, name: 'Target Practice Graphic', category: 'STREETWEAR', price: '20000', rating: '4.9 (42)', image: '/images/IMG_2956.png', tag: 'Indispensable' }
]);

// États de sélection et UI
const selectedProduct = ref(products.value[0]);
const showFilters = ref(false); 
const showToast = ref(false);
const selectedSize = ref('M');
const sizes = ['XS', 'S', 'M', 'L', 'XL', 'XXL'];

// Logique du Panier
const cartItems = ref([]);
const showCart = ref(false);

// WhatsApp Config
const whatsappNumber = "22900000000"; 

// 1. Partager un produit seul
const shareProduct = () => {
  if (navigator.share) {
    navigator.share({
      title: 'Happy Cotton',
      text: `Regarde ce t-shirt ${selectedProduct.value.name} !`,
      url: window.location.href,
    }).catch(() => {});
  } else {
    alert("Lien copié !");
  }
};

// 2. Envoyer tout le panier par WhatsApp
const sendCartToWhatsApp = () => {
  let message = `*Nouvelle Commande - Happy Cotton*%0A%0A`;
  cartItems.value.forEach((item, index) => {
    message += `${index + 1}. ${item.name} (Taille: ${item.chosenSize}) - ${item.price} FCFA%0A`;
  });
  message += `%0A*TOTAL : ${totalPrice.value} FCFA*`;
  
  window.open(`https://wa.me/${whatsappNumber}?text=${message}`, '_blank');
};

const cartCount = computed(() => cartItems.value.length);
const totalPrice = computed(() => cartItems.value.reduce((sum, item) => sum + parseFloat(item.price), 0));

const addToCart = () => {
  cartItems.value.push({ ...selectedProduct.value, chosenSize: selectedSize.value, cartId: Date.now() });
  showToast.value = true;
  setTimeout(() => { showToast.value = false; }, 3000);
};

const removeItem = (id) => { cartItems.value = cartItems.value.filter(item => item.cartId !== id); };

// Lien WhatsApp flottant (Produit actuel)
const whatsappLink = computed(() => {
  const msg = encodeURIComponent(`Bonjour Happy Cotton, je souhaite commander : ${selectedProduct.value.name}`);
  return `https://wa.me/${whatsappNumber}?text=${msg}`;
});
</script>

<template>
  <div class="max-w-md mx-auto bg-white min-h-screen shadow-2xl relative overflow-x-hidden font-sans text-zinc-900">
    
    <a :href="whatsappLink" target="_blank" class="fixed bottom-6 right-6 z-[55] bg-[#25D366] w-14 h-14 rounded-full shadow-2xl flex items-center justify-center animate-bounce-slow border-2 border-white transition-transform active:scale-90">
      <svg width="28" height="28" viewBox="0 0 24 24" fill="white"><path d="M17.472 14.382c-.297-.149-1.758-.867-2.03-.967-.273-.099-.471-.148-.67.15-.197.297-.767.966-.94 1.164-.173.199-.347.223-.644.075-.297-.15-1.255-.463-2.39-1.475-.883-.788-1.48-1.761-1.653-2.059-.173-.297-.018-.458.13-.606.134-.133.298-.347.446-.52.149-.174.198-.298.298-.497.099-.198.05-.371-.025-.52-.075-.149-.669-1.612-.916-2.207-.242-.579-.487-.5-.669-.51-.173-.008-.371-.01-.57-.01-.198 0-.52.074-.792.372-.272.297-1.04 1.016-1.04 2.479 0 1.462 1.065 2.875 1.213 3.074.149.198 2.096 3.2 5.077 4.487.709.306 1.262.489 1.694.625.712.227 1.36.195 1.871.118.571-.085 1.758-.719 2.006-1.413.248-.694.248-1.289.173-1.413-.074-.124-.272-.198-.57-.347m-5.421 7.403h-.004a9.87 9.87 0 01-5.031-1.378l-.361-.214-3.741.982.998-3.648-.235-.374a9.86 9.86 0 01-1.51-5.26c.001-5.45 4.436-9.884 9.888-9.884 2.64 0 5.122 1.03 6.988 2.898a9.825 9.825 0 012.893 6.994c-.003 5.45-4.437 9.884-9.885 9.884m8.413-18.297A11.815 11.815 0 0012.05 0C5.414 0 0 5.414 0 12.05c0 2.123.55 4.197 1.594 6.02L0 24l6.117-1.605a12.03 12.03 0 005.933 1.554h.005c6.635 0 12.05-5.414 12.05-12.05a11.85 11.85 0 00-3.526-8.502z"/></svg>
    </a>

    <header class="flex justify-between items-center p-4 sticky top-0 bg-white/80 backdrop-blur-md z-40 border-b border-zinc-50">
      <div class="flex items-center gap-3">
        <span class="text-xl cursor-pointer hover:opacity-60 transition" @click="showFilters = true">☰</span>
        <h1 class="font-bold text-lg tracking-tighter uppercase leading-none">
          Happy<span class="block text-[8px] font-light tracking-[0.3em]">Cotton</span>
        </h1>
      </div>
      <div @click="showCart = true" class="bg-black text-white px-4 py-2 rounded-full text-[10px] font-bold flex items-center gap-2 relative cursor-pointer active:scale-95 transition">
        👜 Panier
        <span v-if="cartCount > 0" class="absolute -top-1 -right-1 bg-red-500 text-white w-4 h-4 rounded-full flex items-center justify-center text-[8px]">
          {{ cartCount }}
        </span>
      </div>
    </header>

    <Transition name="fade">
      <div v-if="showFilters" class="fixed inset-0 bg-black/60 z-[100] backdrop-blur-sm" @click="showFilters = false"></div>
    </Transition>
    <Transition name="slide-left">
      <div v-if="showFilters" class="fixed top-0 left-0 w-72 h-full bg-white z-[101] p-6 shadow-2xl flex flex-col">
        <div class="flex justify-between items-center mb-10">
          <h2 class="font-black uppercase italic text-xl">Menu</h2>
          <button @click="showFilters = false" class="text-2xl">✕</button>
        </div>
        <nav class="space-y-6">
          <p v-for="cat in ['VINTAGE', 'STREETWEAR', 'ART', 'MINIMAL', 'DARK']" :key="cat" class="text-sm font-black uppercase tracking-widest border-b border-zinc-100 pb-2 cursor-pointer">{{ cat }}</p>
        </nav>
        <div class="mt-auto space-y-4">
          <a href="https://tiktok.com/@anifowose797" target="_blank" class="block text-xs font-black uppercase bg-zinc-100 p-3 rounded-xl text-center">TikTok</a>
          <a href="https://instagram.com/happycotton" target="_blank" class="block text-xs font-black uppercase bg-zinc-100 p-3 rounded-xl text-center">Instagram</a>
        </div>
      </div>
    </Transition>

    <section class="p-4">
      <div class="bg-[#1a1a1a] rounded-[2.5rem] p-8 text-white relative overflow-hidden shadow-xl">
        <h2 class="text-3xl font-bold leading-tight mb-2 italic">Collection <br> Premium 2026</h2>
        <p class="text-gray-400 text-xs mb-6">Des t-shirts d'exception en coton bio.</p>
        <div class="space-y-2 text-[9px] font-bold uppercase tracking-widest text-gray-300">
          <p><span class="text-green-500">✓</span> Livraison dès 2000 FCFA</p>
          <p><span class="text-green-500">✓</span> Coton bio certifié</p>
          <p class="flex items-center gap-2"><span class="text-green-500">✓</span> Retours gratuits 20 jours</p>
        </div>
      </div>
    </section>

    <main class="p-4">
      <div class="grid grid-cols-2 gap-4 mb-10">
        <div v-for="item in products" :key="item.id" @click="selectedProduct = item" class="group cursor-pointer">
          <div class="relative aspect-[3/4] bg-gray-100 rounded-[2rem] overflow-hidden mb-3 border border-transparent transition-all" :class="selectedProduct.id === item.id ? 'border-zinc-900 ring-1 ring-zinc-900' : ''">
            <img :src="item.image" class="w-full h-full object-cover group-hover:scale-105 transition duration-500" />
            <div v-if="item.tag" class="absolute top-3 left-3 bg-white text-[7px] font-black px-2 py-1 rounded shadow-sm uppercase italic">{{ item.tag }}</div>
          </div>
          <h4 class="font-bold text-[11px] truncate uppercase tracking-tighter">{{ item.name }}</h4>
          <span class="font-black text-xs">{{ item.price }} FCFA</span>
        </div>
      </div>
    </main>

    <section class="mx-4 p-6 bg-zinc-50 rounded-[2.5rem] border border-zinc-100 mb-10">
      <div class="flex justify-between items-start mb-4">
        <h3 class="text-lg font-black italic uppercase leading-tight">{{ selectedProduct.name }}</h3>
        <button @click="shareProduct" class="p-2 bg-white rounded-full border border-zinc-200 shadow-sm active:scale-90 transition">
          <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><circle cx="18" cy="5" r="3"/><circle cx="6" cy="12" r="3"/><circle cx="18" cy="19" r="3"/><line x1="8.59" y1="13.51" x2="15.42" y2="17.49"/><line x1="15.41" y1="6.51" x2="8.59" y2="10.49"/></svg>
        </button>
      </div>

      <div class="flex flex-wrap gap-2 mb-6">
        <button v-for="size in sizes" :key="size" @click="selectedSize = size"
          :class="selectedSize === size ? 'bg-black text-white' : 'bg-white text-black border-zinc-200'"
          class="w-10 h-10 rounded-xl border font-bold text-[10px] transition-all">{{ size }}</button>
      </div>
      <button @click="addToCart" class="w-full bg-zinc-900 text-white py-4 rounded-2xl font-black uppercase text-[10px] tracking-widest shadow-lg active:scale-95 transition">
        Ajouter au panier — {{ selectedProduct.price }} FCFA
      </button>
    </section>

   <footer class="bg-zinc-900 text-white rounded-t-[3rem] p-10 mt-20 text-center">
      <h2 class="font-black text-xl italic uppercase tracking-tighter">Happy Cotton</h2>
      <p class="text-[9px] text-zinc-500 font-bold uppercase tracking-[0.2em] mt-2 mb-8">Est 2026</p>
      
      <div class="flex justify-center gap-8 mb-10">
        <a href="https://instagram.com/happycotton" target="_blank" class="flex items-center gap-2 text-[10px] font-black uppercase text-zinc-400 hover:text-white transition">
          <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="2" y="2" width="20" height="20" rx="5" ry="5"></rect><path d="M16 11.37A4 4 0 1 1 12.63 8 4 4 0 0 1 16 11.37z"></path><line x1="17.5" y1="6.5" x2="17.51" y2="6.5"></line></svg>
          Instagram
        </a>
        
        <a href="https://tiktok.com/@anifowose797" target="_blank" class="flex items-center gap-2 text-[10px] font-black uppercase text-zinc-400 hover:text-white transition">
          <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M9 12a4 4 0 1 0 4 4V4a5 5 0 0 0 5 5"></path></svg>
          TikTok
        </a>
      </div>

      <div class="grid grid-cols-2 gap-10 mb-10 border-y border-zinc-800 py-10">
        <div class="space-y-4">
          <h4 class="text-[10px] font-black uppercase text-zinc-400 tracking-widest">Navigation</h4>
          <ul class="text-[11px] font-bold space-y-2 uppercase text-zinc-200">
            <li class="hover:text-white cursor-pointer">Accueil</li>
            <li class="hover:text-white cursor-pointer">Collections</li>
            <li class="hover:text-white cursor-pointer">À propos</li>
          </ul>
        </div>
        <div class="space-y-4">
          <h4 class="text-[10px] font-black uppercase text-zinc-400 tracking-widest">Support</h4>
          <ul class="text-[11px] font-bold space-y-2 uppercase text-zinc-200">
            <li class="hover:text-white cursor-pointer">Livraison</li>
            <li class="hover:text-white cursor-pointer">Retours</li>
            <li class="hover:text-white cursor-pointer">Contact</li>
          </ul>
        </div>
      </div>

      <div class="flex flex-wrap justify-center gap-6 opacity-50">
        <div class="flex items-center gap-1.5">
          <span class="text-xs">⚡</span>
          <span class="text-[8px] font-bold uppercase tracking-widest">Livraison Express</span>
        </div>
        <div class="flex items-center gap-1.5">
          <span class="text-xs">🔒</span>
          <span class="text-[8px] font-bold uppercase tracking-widest">Paiement Sécurisé</span>
        </div>
        <div class="flex items-center gap-1.5">
          <span class="text-xs">🌿</span>
          <span class="text-[8px] font-bold uppercase tracking-widest">100% Coton Bio</span>
        </div>
      </div>
    </footer>

    <Transition name="slide">
      <div v-if="showCart" class="fixed inset-0 bg-black/60 z-[60] flex justify-end" @click.self="showCart = false">
        <div class="w-full max-w-[300px] bg-white h-full shadow-2xl flex flex-col p-6">
           <div class="flex justify-between items-center mb-6">
            <h2 class="font-black uppercase italic tracking-tighter text-xl">Ton Panier</h2>
            <button @click="showCart = false" class="text-2xl">✕</button>
          </div>
          <div class="flex-1 overflow-y-auto space-y-4">
            <div v-if="cartItems.length === 0" class="text-center py-20 text-zinc-400 text-[10px] font-black uppercase italic">Panier vide</div>
            <div v-for="item in cartItems" :key="item.cartId" class="flex gap-4 bg-zinc-50 p-3 rounded-2xl">
              <img :src="item.image" class="w-14 h-18 object-cover rounded-xl" />
              <div class="flex-1">
                <h4 class="text-[9px] font-black uppercase leading-tight">{{ item.name }}</h4>
                <p class="text-xs font-black mt-1">{{ item.price }} FCFA ({{ item.chosenSize }})</p>
              </div>
              <button @click="removeItem(item.cartId)" class="text-red-500 font-bold">✕</button>
            </div>
          </div>
          <div v-if="cartItems.length > 0" class="pt-6 border-t bg-white">
            <div class="flex justify-between font-black uppercase text-xs italic mb-4"><span>Total</span><span>{{ totalPrice }} FCFA</span></div>
            <button @click="sendCartToWhatsApp" class="w-full bg-[#25D366] text-white py-4 rounded-2xl font-black uppercase text-[10px] tracking-widest shadow-lg flex items-center justify-center gap-2">
              Commander via WhatsApp 📱
            </button>
          </div>
        </div>
      </div>
    </Transition>

    <Transition name="toast">
      <div v-if="showToast" class="fixed bottom-10 left-4 right-4 z-[100]">
        <div class="bg-zinc-900 text-white p-4 rounded-2xl shadow-2xl flex items-center gap-4 border border-white/10">
          <div class="bg-green-500 w-8 h-8 rounded-full flex items-center justify-center text-xs">✓</div>
          <p class="text-[10px] font-black uppercase tracking-widest">Article ajouté !</p>
        </div>
      </div>
    </Transition>
    
  </div>
</template>

<style scoped>
.slide-left-enter-active, .slide-left-leave-active { transition: all 0.4s cubic-bezier(0.16, 1, 0.3, 1); }
.slide-left-enter-from, .slide-left-leave-to { transform: translateX(-100%); opacity: 0; }

.slide-enter-active, .slide-leave-active { transition: all 0.4s ease; }
.slide-enter-from, .slide-leave-to { transform: translateX(100%); opacity: 0; }

.fade-enter-active, .fade-leave-active { transition: opacity 0.3s ease; }
.fade-enter-from, .fade-leave-to { opacity: 0; }

.toast-enter-active, .toast-leave-active { transition: all 0.5s cubic-bezier(0.16, 1, 0.3, 1); }
.toast-enter-from { transform: translateY(100px); opacity: 0; }
.toast-leave-to { transform: scale(0.9); opacity: 0; }

@keyframes bounce-slow { 0%, 100% { transform: translateY(0); } 50% { transform: translateY(-10px); } }
.animate-bounce-slow { animation: bounce-slow 3s infinite ease-in-out; }
</style>