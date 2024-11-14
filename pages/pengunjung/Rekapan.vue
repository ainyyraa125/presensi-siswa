<template>
  <div class="container-fluid">
    <div class="my-3">
      <nuxt-link to="/">
        <button type="button" class="btn btn-lg btn-secondary radius kembali" style="float: right;">KEMBALI</button>
      </nuxt-link>  
      <h2 class="text-center my-4">REKAP PRESENSI PER MINGGU</h2>
      <label for="weekSelect" class="form-label">Pilih Minggu:</label>
      <select
        id="weekSelect"
        v-model="selectedWeek"
        @change="filterDataByWeek"
        class="form-select"
      >
        <option v-for="week in weeks" :key="week" :value="week">{{ week }}</option>
      </select>
    </div>

    <div v-if="loading" class="text-center">Loading data...</div>

    <div v-else>
      <div class="table-responsive">
        <table class="table table-bordered">
          <thead>
            <tr>
              <th scope="col">NAMA</th>
              <th scope="col">MINGGU</th>
              <th scope="col">JUMLAH HADIR</th>
              <th scope="col">JUMLAH IZIN</th>
              <th scope="col">JUMLAH SAKIT</th>
              <th scope="col">JUMLAH ALFA</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(rekap, index) in filteredRekapData" :key="index">
              <td>{{ rekap.nama }}</td>
              <td>{{ rekap.minggu }}</td>
              <td>{{ rekap.jumlahHadir }}</td>
              <td>{{ rekap.jumlahIzin }}</td>
              <td>{{ rekap.jumlahSakit }}</td>
              <td>{{ rekap.jumlahAlfa }}</td>
            </tr>
            <tr v-if="filteredRekapData.length === 0">
              <td colspan="6" class="text-center">Tidak ada data tersedia</td>
            </tr>
          </tbody>
        </table>
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
      content: "Halaman REKAPAN PERMINGGU",
    },
  ],
});
import { ref, onMounted } from 'vue';
const supabase = useSupabaseClient();
const loading = ref(true);
const rekapData = ref([]);
const filteredRekapData = ref([]);
const selectedWeek = ref("");
const weeks = ref([]);

// Fungsi untuk mengambil data rekap presensi per minggu
const fetchRekapData = async () => {
  try {
    loading.value = true;

    const { data: presensiData, error } = await supabase
      .from("Presensi")
      .select(`
        tanggal,
        Siswa (nama),
        Statuskehadiran (keterangan)
      `);

    if (error) throw error;

    const rekap = {};

    presensiData.forEach((presensi) => {
      const date = new Date(presensi.tanggal);
      const year = date.getFullYear();
      const week = getWeekNumber(date);
      const minggu = `${year}-W${week}`;
      const key = `${presensi.Siswa.nama}-${minggu}`;

      if (!rekap[key]) {
        rekap[key] = {
          minggu,
          nama: presensi.Siswa.nama,
          jumlahHadir: 0,
          jumlahIzin: 0,
          jumlahSakit: 0,
          jumlahAlfa: 0,
        };
      }

      const keterangan = presensi.Statuskehadiran?.keterangan?.toLowerCase().trim();

      switch (keterangan) {
        case "hadir":
          rekap[key].jumlahHadir++;
          break;
        case "izin":
          rekap[key].jumlahIzin++;
          break;
        case "sakit":
          rekap[key].jumlahSakit++;
          break;
        case "alfa":
          rekap[key].jumlahAlfa++;
          break;
        default:
          // Coba perbaiki kesalahan ejaan
          if (keterangan === "alpa") {
            rekap[key].jumlahAlfa++;
            console.warn(`Status diperbaiki: ${keterangan} menjadi alfa`);
          } else {
            console.warn(`Status tidak dikenal: ${keterangan}`);
          }
      }
    });

    rekapData.value = Object.values(rekap);
    weeks.value = [...new Set(rekapData.value.map((r) => r.minggu))];
    filterDataByWeek();
  } catch (error) {
    console.error("Error fetching data:", error);
  } finally {
    loading.value = false;
  }
};

// Fungsi untuk mendapatkan nomor minggu dari tanggal
const getWeekNumber = (date) => {
  const firstDayOfYear = new Date(date.getFullYear(), 0, 1);
  const pastDaysOfYear = (date - firstDayOfYear) / 86400000;
  return Math.ceil((pastDaysOfYear + firstDayOfYear.getDay() + 1) / 7);
};

// Fungsi untuk filter data berdasarkan minggu yang dipilih
const filterDataByWeek = () => {
  if (selectedWeek.value) {
    filteredRekapData.value = rekapData.value.filter(
      (rekap) => rekap.minggu === selectedWeek.value
    );
  } else {
    filteredRekapData.value = rekapData.value;
  }
};

// Ambil data saat komponen dimuat
onMounted(() => {
  fetchRekapData();
});
</script>

<style scoped>
.table {
  margin-top: 20px;
}

.table-responsive {
  overflow-x: auto;
}

@media (max-width: 767px) {
  .table th,
  .table td {
    white-space: nowrap;
  }
}
</style>
