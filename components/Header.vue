<template>
  <div class="row p-3 bg-primary text-white align-items-center">
    <div class="col-12 col-lg-1 d-flex justify-content-center justify-content-lg-start">
      <img src="~/assets/img/LOGO-SMK4 (2).png" alt="Logo SMK Negeri 4 Tasikmalaya" />
    </div>

    <div class="col-12 col-lg-10 text-center text-lg-start">
      <h1>PRESENSI SISWA</h1>
      <h2>XII PPLG 2</h2>
      <h3>SMK Negeri 4 Tasikmalaya</h3>
    </div>

    <div class="col-12 col-lg-1 d-flex justify-content-center justify-content-lg-end">
      <img src="~/assets/img/logo-jurusan.png" alt="Logo Jurusan" />
    </div>
  </div>
</template>

<style scoped>
img {
  width: 100%;
  max-width: 150px; /* Mengurangi ukuran gambar untuk tampilan lebih baik */
  height: auto;
}

h1, h2, h3 {
  font-size: 20px; /* Ukuran font lebih kecil untuk mobile */
}

@media (min-width: 768px) {
  h1, h2, h3 {
    font-size: 25px; /* Ukuran font yang lebih besar untuk layar besar */
  }
}
</style>
