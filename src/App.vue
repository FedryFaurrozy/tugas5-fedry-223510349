<template>
  <q-layout
    view="hHh lpR fFf"
    :style="{
      backgroundImage:
        'url(https://w0.peakpx.com/wallpaper/98/976/HD-wallpaper-orange-graphic-orange-colour-background.jpg)',
    }"
  >
    <!-- Navbar -->
    <q-header elevated class="bg-orange-7 text-black">
      <q-toolbar>
        <img
          src="https://sin1.contabostorage.com/0a986eb902c4469cb860e43985eb18a1:vocapanel/3ztstore/20220609_094857-9f62-original.png"
          alt="Logo Toko Online"
          style="height: 50px"
        />
        <q-toolbar-title class="text-black"> Fedry X 3ztstore </q-toolbar-title>
        <q-space></q-space>
        <q-input
          outlined
          v-model="search"
          dense
          placeholder="Cari..."
          class="q-mr-sm text-black"
          :style="{ maxWidth: '300px' }"
          v-show="showSearch"
        />
        <q-btn icon="search" flat @click="toggleSearch" class="text-black" />
        <q-btn flat @click="showLoginDialog" class="text-black">
          <q-icon name="login" class="text-black" />
          <span class="q-ml-sm">Login / Register</span>
        </q-btn>
      </q-toolbar>
    </q-header>

    <!-- Enhanced Dialog for login with a more attractive design -->
    <q-dialog v-model="loginDialog" full-width full-height>
      <q-card
        class="bg-orange-7 text-black"
        style="backdrop-filter: blur(15px); border-radius: 20px"
      >
        <q-card-section class="row items-center q-pb-none">
          <q-space></q-space>
          <q-btn icon="close" flat round @click="loginDialog = false" />
        </q-card-section>
        <q-card-section class="q-pt-none">
          <div class="text-h5 text-center">Welcome Back!</div>
          <div class="text-subtitle2 text-center q-mb-md">
            Login to continue
          </div>
          <q-input filled v-model="username" label="Username" class="q-ma-md" />
          <q-input
            filled
            v-model="password"
            label="Password"
            type="password"
            class="q-ma-md"
          />
          <div class="text-center q-mt-md">
            <q-btn
              label="Login"
              color="orange"
              class="full-width"
              @click="login"
            />
            <div class="q-mt-md row justify-center q-gutter-sm">
              <q-btn
                icon="img:icons/google.svg"
                flat
                @click="loginWithGoogle"
              />
              <q-btn
                icon="img:icons/facebook.svg"
                flat
                @click="loginWithFacebook"
              />
            </div>
          </div>
        </q-card-section>
      </q-card>
    </q-dialog>

    <!-- Navbar tambahan di bawah header, turun lebih jauh -->
    <q-toolbar
      class="bg-orange-8 text-black"
      style="margin-top: 50px; height: 40px"
    >
      <q-toolbar class="justify-center">
        <q-btn flat label="Beranda" icon="home" class="custom-btn" />
        <q-btn flat label="Semua Game" icon="games" class="custom-btn" />
        <q-btn
          flat
          label="Lacak Pesanan"
          icon="local_shipping"
          class="custom-btn"
        />
        <q-btn flat label="Pricelist" icon="attach_money" class="custom-btn" />
        <q-btn
          flat
          label="Kalkulator MLBB"
          icon="calculate"
          class="custom-btn"
        />
        <q-btn flat label="Ulasan" icon="rate_review" class="custom-btn" />
        <q-btn flat label="Berita" icon="announcement" class="custom-btn" />
        <q-btn
          flat
          label="Beli Followers"
          icon="group_add"
          class="custom-btn"
        />
        <q-btn
          flat
          label="Join Reseller"
          icon="storefront"
          class="custom-btn"
        />
      </q-toolbar>
    </q-toolbar>

    <!-- Tampilkan satu gambar besar -->
    <div
      class="shadow-2 rounded-borders"
      style="
        height: 500px;
        margin-top: 20px;
        border-radius: 15px;
        overflow: hidden;
        background-image: url('https://sin1.contabostorage.com/0a986eb902c4469cb860e43985eb18a1:vocapanel/3ztstore/3zt2-1fb4-original.jpeg');
        background-size: contain;
        background-position: center;
        background-repeat: no-repeat;
      "
    ></div>

    <!-- Produk yang dapat diklik dengan efek blur pada teks -->
    <div class="product-container">
      <div
        v-for="product in products"
        :key="product.id"
        class="product"
        @click="addToCart(product)"
      >
        <img :src="product.image" :alt="product.name" class="product-image" />
        <div class="product-description">
          <span>{{ product.description }}</span>
        </div>
      </div>
    </div>

    <!-- Footer -->
    <q-footer
      class="bg-orange-9 text-black"
      style="position: relative; z-index: 10"
    >
      <q-toolbar class="justify-center">
        <q-toolbar-title class="text-center text-black">
          © 2024 Fedry X 3ztstore. All rights reserved.
        </q-toolbar-title>
      </q-toolbar>
    </q-footer>
  </q-layout>
</template>

<script>
export default {
  data() {
    return {
      showSearch: false,
      search: "",
      slide: 1,
      carouselImages: [
        "https://sin1.contabostorage.com/0a986eb902c4469cb860e43985eb18a1:vocapanel/3ztstore/3zt2-1fb4-original.jpeg",
        "https://sin1.contabostorage.com/0a986eb902c4469cb860e43985eb18a1:vocapanel/3ztstore/IMG_4537-c53b-original.png",
        "https://sin1.contabostorage.com/0a986eb902c4469cb860e43985eb18a1:vocapanel/3ztstore/3zt12%20(2)%20(1)-d5a2-original.png",
        "https://sin1.contabostorage.com/0a986eb902c4469cb860e43985eb18a1:vocapanel/3ztstore/3zt39-d6fe-original.jpeg",
        "https://sin1.contabostorage.com/0a986eb902c4469cb860e43985eb18a1:vocapanel/3ztstore/3zt5-db79-original.jpeg",
      ],
      products: [
        // Data produk tetap dipertahankan
        {
          id: 1,
          name: "Produk 1",
          description: "Mobile Legends",
          image:
            "https://sin1.contabostorage.com/0a986eb902c4469cb860e43985eb18a1:vocapanel/3ztstore/NEW2-6d3d-original.jpeg",
        },
        {
          id: 2,
          name: "Produk 2",
          description: "Free Fire Indonesia",
          image:
            "https://sin1.contabostorage.com/0a986eb902c4469cb860e43985eb18a1:vocapanel/3ztstore/NEW1%20(2)-652e-original.jpg",
        },
        {
          id: 3,
          name: "Produk 3",
          description: "Honor Of Kings",
          image:
            "https://sin1.contabostorage.com/0a986eb902c4469cb860e43985eb18a1:vocapanel/3ztstore/NEW6%20(2)-4bb7-original.jpg",
        },
        {
          id: 4,
          name: "Produk 4",
          description: "PUBG Mobile",
          image:
            "https://sin1.contabostorage.com/0a986eb902c4469cb860e43985eb18a1:vocapanel/3ztstore/NEW7-debd-original.jpg",
        },
        {
          id: 5,
          name: "Produk 5",
          description: "Arena Breakout",
          image:
            "https://sin1.contabostorage.com/0a986eb902c4469cb860e43985eb18a1:vocapanel/3ztstore/NEW8-7a05-original.jpg",
        },
        // Menambahkan 10 produk lagi
        {
          id: 6,
          name: "Produk 6",
          description: "Clash Royale",
          image:
            "https://sin1.contabostorage.com/0a986eb902c4469cb860e43985eb18a1:vocapanel/3ztstore/NEW37-f87c-original.jpg",
        },
        {
          id: 7,
          name: "Produk 7",
          description: "Metal Slug: Awakening",
          image:
            "https://sin1.contabostorage.com/0a986eb902c4469cb860e43985eb18a1:vocapanel/3ztstore/NEW10-7c52-original.jpg",
        },
        {
          id: 8,
          name: "Produk 8",
          description: "Ghensin Impact",
          image:
            "https://sin1.contabostorage.com/0a986eb902c4469cb860e43985eb18a1:vocapanel/3ztstore/NEW11-91bc-original.jpg",
        },
        {
          id: 9,
          name: "Produk 9",
          description: "Honkai Star Rail",
          image:
            "https://sin1.contabostorage.com/0a986eb902c4469cb860e43985eb18a1:vocapanel/3ztstore/NEW12-2d45-original.jpg",
        },
        {
          id: 10,
          name: "Produk 10",
          description: "Honkai Impact 3",
          image:
            "https://sin1.contabostorage.com/0a986eb902c4469cb860e43985eb18a1:vocapanel/3ztstore/NEW13-aa06-original.jpg",
        },
        {
          id: 11,
          name: "Produk 11",
          description: "Undawn",
          image:
            "https://sin1.contabostorage.com/0a986eb902c4469cb860e43985eb18a1:vocapanel/3ztstore/NEW14-2a51-original.jpg",
        },
        {
          id: 12,
          name: "Produk 12",
          description: "Night Crows",
          image:
            "https://sin1.contabostorage.com/0a986eb902c4469cb860e43985eb18a1:vocapanel/3ztstore/NEW15-e6110-original.jpg",
        },
        {
          id: 13,
          name: "Produk 13",
          description: "Call Of Dutty Mobile",
          image:
            "https://sin1.contabostorage.com/0a986eb902c4469cb860e43985eb18a1:vocapanel/3ztstore/NEW33-3d7d-original.jpg",
        },
        {
          id: 14,
          name: "Produk 14",
          description: "Valorant",
          image:
            "https://sin1.contabostorage.com/0a986eb902c4469cb860e43985eb18a1:vocapanel/3ztstore/NEW34-de8f-original.jpg",
        },
        {
          id: 15,
          name: "Produk 15",
          description: "Clash Of Clans",
          image:
            "https://sin1.contabostorage.com/0a986eb902c4469cb860e43985eb18a1:vocapanel/3ztstore/NEW36-91102-original.jpg",
        },
      ],
      loginDialog: false,
      username: "",
      password: "",
    };
  },
  methods: {
    toggleSearch() {
      this.showSearch = !this.showSearch;
    },
    showLoginDialog() {
      this.loginDialog = true;
    },
    login() {
      // Implement login logic or redirect to login page
      console.log("Login attempt with username:", this.username);
    },
    loginWithGoogle() {
      // Redirect to Google login
      console.log("Redirecting to Google login");
    },
    loginWithFacebook() {
      // Redirect to Facebook login
      console.log("Redirecting to Facebook login");
    },
    addToCart(item) {
      // Implement add to cart logic
      console.log("Added to cart:", item);
    },
  },
};
</script>

<style>
.bg-blur {
  backdrop-filter: blur(10px);
}

.custom-btn {
  font-size: 12px;
  color: black;
}
.product-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
  padding: 20px;
  gap: 20px;
}
.product {
  cursor: pointer;
  width: 100%; /* Mengubah lebar menjadi 100% untuk responsifitas */
  max-width: 200px; /* Menetapkan lebar maksimum */
  height: auto; /* Mengatur tinggi menjadi otomatis */
  position: relative;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  backdrop-filter: blur(8px); /* Menambahkan efek blur */
}
.product:hover {
  transform: translateY(-10px);
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
}
.product-image {
  width: 100%;
  height: 80%;
  display: block;
  object-fit: cover;
}
.product-description {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 20%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  font-size: 16px;
  text-align: center;
  backdrop-filter: blur 5px;
}
</style>
