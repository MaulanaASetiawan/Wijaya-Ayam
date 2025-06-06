<script setup>
import { Head, Link, usePage, router } from '@inertiajs/vue3';
import Footer from '@/Components/Footer.vue';
import Header from '@/Components/Header.vue';
import { ref } from 'vue';

const props = defineProps({
  canLogin: Boolean,
  canRegister: Boolean,
});

const { products } = usePage().props;
const cartKey = ref(0); // Buat reload/re-render header

function addToCart(itemId) {
    router.post(`/cart/add/${itemId}`, {}, {
      onFinish: () => {
        // router.visit(route('home'),  {preserveScroll: true})
        cartKey.value++; // biar heeader nya reload pas proses selesai
      },
      preserveScroll: true, // nyimpan scroll pas update data, bukan buat visit atau get ke halaman sendiri / halaman lain
    })
  }
</script>

<!-- Alternativ, bikin propsnya di script laen soalnya gak bisa di setup -->
<!-- <script>
// export default {
//     name: 'Home',
//     components: {
//         Header,
//     },
//     props: {
//         canLogin: Boolean,
//         canRegister: Boolean,
//     },
// };
</script> -->

<template>
  <div class="home">
    <Header :key="cartKey" activePage="home" />
    <section class="hero bg-home relative bg-cover bg-center h-screen">
      <div class="absolute inset-0 bg-gradient-to-r from-black via-transparent to-black opacity-70 z-0"></div>
      <div class="container mx-auto px-4 md:px-20 flex items-center justify-start h-full z-10">
        <div class="text-white max-w-lg">
          <h1 class="text-4xl md:text-6xl font-bold leading-tight">
            Segar, Bersih, dan Siap Dimasak!
          </h1>
          <p class="mt-4 text-lg md:text-xl">
            Nikmati daging ayam potong berkualitas terbaik untuk keluarga Anda. Pesan sekarang dan rasakan bedanya!
          </p>
          <div class="mt-6">
            <Link :href="route('product')">
              <button class="btnStyle bg-amber-300 hover:bg-amber-400 text-black font-semibold px-6 py-3 rounded shadow-md transition">
                Pesan Sekarang
              </button>
            </Link >
          </div>
        </div>
      </div>
    </section>

    <section class="py-16 bg-white">
      <div class="container mx-auto px-4 text-center justify-center">
        <h2 class="text-3xl font-bold text-gray-800 mb-6">Mengapa Harus Memilih Kami?</h2>
        <div class="grid grid-cols-1 sm:grid-cols-3 text-center justify-center">
          <div class="p-4">
            <img src="../assets/tree.png" alt="Icon" class="w-16 h-16 mx-auto mb-4" />
            <h3 class="text-xl font-semibold text-gray-800">Segar Setiap Hari</h3>
            <p class="text-gray-600 mt-2">
              Kami memastikan ayam selalu segar untuk anda.
            </p>
          </div>
          <div class="p-4">
            <img src="../assets/sparkles.png" alt="Icon" class="w-16 h-16 mx-auto mb-4" />
            <h3 class="text-xl font-semibold text-gray-800">Pengolahan Higienis</h3>
            <p class="text-gray-600 mt-2">
              Proses pengolahan kami terjamin kebersihannya.
            </p>
          </div>
          <div class="p-4">
            <img src="../assets/time-fast.png" alt="Icon" class="w-16 h-16 mx-auto mb-4" />
            <h3 class="text-xl font-semibold text-gray-800">Pengiriman Cepat</h3>
            <p class="text-gray-600 mt-2">
              Pesanan Anda tiba dengan cepat di depan pintu.
            </p>
          </div>
        </div>
      </div>
    </section>

    <section class="py-16 bg-gray-100">
      <div class="container mx-auto px-4">
        <div class="text-center mb-12">
          <h2 class="text-3xl font-bold text-gray-800">Produk Kami</h2>
          <p class="text-gray-600 mt-2">
            Pilihan ayam potong segar dengan berbagai ukuran dan jenis untuk kebutuhan Anda.
          </p>
        </div>

        <div class="grid grid-cols-1 xs:grid-cols-2 lg:grid-cols-4 gap-8">
          <div v-for="product in products.slice(0, 4)" class="bg-white rounded-lg shadow-md overflow-hidden">
            <img :src="`storage/${product.gambar}`" alt="Ayam Utuh" class="w-full h-48 object-cover" />
            <div class="flex flex-col p-4">
              <h3 class="text-xl font-semibold text-gray-800">RP. {{ product.harga.toLocaleString() }}</h3>
              <h3 class="text-xl font-semibold text-gray-800">{{ product.nama_produk }}</h3>
              <p class="text-gray-600 mt-2">
                {{ product.deskripsi }}
              </p>
              <button v-if="$page.props.auth.user" @click="addToCart(product.id_produk)" class="btnStyle self-end mt-4 bg-green-500 hover:bg-green-600 text-white font-semibold px-4 py-2 rounded transition">
                + Keranjang
              </button>
              <Link v-else :href="route('login')" 
                class="btnStyle self-end mt-4 bg-green-500 hover:bg-green-600 text-white font-semibold px-4 py-2 rounded transition">
                + Keranjang
            </Link>
            </div>
          </div>

        </div>
      </div>
    </section>
    
    <section class="toAbout relative bg-cover bg-center h-[500px] flex items-center justify-center">
    <div class="absolute inset-0 bg-black bg-opacity-50"></div>

    <!-- Content -->
    <div class="relative text-center text-white z-10">
      <h3 class="text-xl uppercase tracking-wide mb-4">Wijaya Ayam</h3>
      <h1 class="text-4xl md:text-6xl font-bold leading-tight mb-6">
        Temukan Kesegaran Ayam Premium Kami
      </h1>
      <Link :href="route('about')" class="bg-white text-black font-medium px-8 py-3 rounded-md hover:bg-gray-200 transition">
        Learn More
    </Link>
    </div>
  </section>

    <section class="py-16 bg-gray-50">
      <div class="container mx-auto px-4">
        <div class="text-center mb-12">
          <h2 class="text-3xl font-bold text-gray-800">Customers Reviews</h2>
        </div>

        <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
          <!-- Review Card 1 -->
          <div class="review bg-white rounded-lg shadow-md p-6">
            <div class="flex justify-center">
              <div class="flex items-center space-x-1 text-yellow-500">
                <i class="fas fa-star"></i>
                <i class="fas fa-star"></i>
                <i class="fas fa-star"></i>
                <i class="fas fa-star"></i>
                <i class="fas fa-star"></i>
              </div>
            </div>
            <p class="text-gray-600 mt-4 text-center">
              "Pemilihan Potongan Daging yang tepat untuk setiap resep sesuai anjuran deskripsi produk, membuat saya merekomendasikan Wijaya Ayam."
            </p>
            <div class="flex items-center justify-center mt-6">
              <img
                class="w-10 h-10 rounded-full"
                src="https://randomuser.me/api/portraits/men/32.jpg"
                alt="Abdul Latif"
              />
              <div class="ml-4">
                <p class="text-gray-800 font-semibold">Abdul Latif</p>
              </div>
            </div>
          </div>

          <!-- Review Card 2 (Promo) -->
          <div class="review bg-black text-white rounded-lg shadow-md p-6 flex flex-col items-center justify-center">
            <h3 class="text-2xl font-bold text-center mb-4">
              Dapatkan Daging ayam Segar Lokal Premium Terbaik
            </h3>
            <p class="text-center text-gray-300">
              Berbelanja daging ayam online di Wijaya Ayam.
            </p>
            <Link :href="route('product')">
              <button class="btnStyle mt-6 bg-green-500 hover:bg-green-600 text-white font-semibold px-6 py-2 rounded shadow-md transition">
                Shop Now →
              </button>
            </Link>
          </div>

          <!-- Review Card 3 -->
          <div class="review bg-white rounded-lg shadow-md p-6">
            <div class="flex justify-center">
              <div class="flex items-center space-x-1 text-yellow-500">
                <i class="fas fa-star"></i>
                <i class="fas fa-star"></i>
                <i class="fas fa-star"></i>
                <i class="fas fa-star"></i>
                <i class="fas fa-star"></i>
              </div>
            </div>
            <p class="text-gray-600 mt-4 text-center">
              "Dagingnya segar dan bagus, saya merekomendasikan Wijaya Ayam untuk kebutuhan Anda."
            </p>
            <div class="flex items-center justify-center mt-6">
              <img
                class="w-10 h-10 rounded-full"
                src="https://randomuser.me/api/portraits/men/44.jpg"
                alt="Ananda Raihan"
              />
              <div class="ml-4">
                <p class="text-gray-800 font-semibold">Ananda Raihan</p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
    <Footer/>
  </div>
</template>

<script>

</script>

<style>
.hero {
  background-size: cover;
  background-position: center;
}

.bg-home {
  background-image: url(../assets/background1.png);
}

.home {
  width: 100%;
  position: absolute;
  top: 0;
}

.review {
  height: 300px;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.btnStyle {
  border-radius: 10px;
  cursor: pointer;
  padding: 10px 20px;
}

.toAbout {
  background-image: url(../assets/background2.png);
}
</style>
