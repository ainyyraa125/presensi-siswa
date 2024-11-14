<template>
  <div class="container-fluid">
    <div class="row my-5 ">
      <div class="col-12  mb-4">
        <nuxt-link to="/login">
          <button type="submit" class="btn btn-lg btn-danger radius kembali">
            Logout
          </button>
        </nuxt-link>
      </div>

      <div class="col-lg-4 col-md-6 col-12 mb-4">
        <nuxt-link to="/pengunjung/tambah">
          <div class="card bg-MASUK rounded-5">
            <div class="card-body text-center">
              <h2>ABSEN MASUK</h2>
            </div>
          </div>
        </nuxt-link>
      </div>

      <div class="col-lg-4 col-md-6 col-12 mb-4">
        <nuxt-link to="/pengunjung/tambah">
          <div class="card bg-KELUAR rounded-5">
            <div class="card-body text-center">
              <h2>ABSEN KELUAR</h2>
            </div>
          </div>
        </nuxt-link>
      </div>

      <div class="col-lg-4 col-md-6 col-12 mb-4">
        <nuxt-link to="/pengunjung/rekapan">
          <div class="card bg-Rekapan rounded-5">
            <div class="card-body text-center">
              <h2>REKAPAN</h2>
            </div>
          </div>
        </nuxt-link>
      </div>
    </div>
  </div>
</template>

<script setup>
useHead({
  title: "PRESENSI",
  meta: [
    {
      name: "description",
      content: "Selamat datang di aplikasi presensi siswa digital",
    },
  ],
});
</script>

<style scoped>
* {
  text-decoration: none;
}
.card {
  height: 250px;
  box-shadow: 1px 1px 10px #424242;
}
.card.bg-MASUK {
  margin-top: 5%;
  background-image: url("../assets/img/absen.png");
  background-repeat: no-repeat;
  background-position: center center;
  background-size: cover;
}
.card.bg-KELUAR {
  margin-top: 5%;
  background-image: url("../assets/img/keluar.png");
  background-repeat: no-repeat;
  background-position: center center;
  background-size: cover;
}
.card.bg-Rekapan {
  margin-top: 5%;
  background-image: url("../assets/img/Rekapan.jpeg");
  background-repeat: no-repeat;
  background-position: center center;
  background-size: cover;
}

h2 {
  color: rgb(34, 115, 207);
  font-family: Arial, sans-serif;
}

@media (max-width: 768px) {
  .card {
    height: 200px;
  }
  .row .col-md-6, .row .col-12 {
    margin-top: 3%;
  }
}
</style>
