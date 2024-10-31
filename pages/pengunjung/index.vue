<template>
  <div class="container-fluid">
    <h2 class="text-center my-4">PRESENSI SISWA</h2>
    <div class="my-3">
      <form @submit.prevent="getpengunjung">
        <input 
          v-model="keyword" 
          type="search" 
          class="form-control form-control-lg rounded-5 border-primary" 
          placeholder="Cari nama kamu disini..." 
          @input="debouncedGetPengunjung" 
        />
      </form>
    </div>
    <nuxt-link to="/pengunjung/tambah">
      <button type="button" class="btn btn-lg btn-secondary radius kembali"  style="float: right;">KEMBALI</button>
    </nuxt-link>    
    <div class="my-3 text-muted">
      <h3>Menampilkan {{ visitors.length }} dari {{ total }}</h3>
    </div>
    
    <table class="table">
      <thead>
        <tr>
          <td>#</td>
          <td>TANGGAL</td>
          <td>HARI</td>
          <td>NAMA</td>
          <td>KEHADIRAN</td>
          <td>OPSI</td>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(visitor, i) in visitors" :key="i">
          <td>{{ i + 1 }}</td>
          <td>{{ visitor.tanggal }}</td>
          <td>{{ visitor.hari?.hari }}</td>
          <td>{{ visitor.siswa?.nama }}</td>
          <td>{{ visitor.keterangan?.keterangan }}</td>
          <td>{{ visitor.opsi }}</td>
        </tr>
        <tr v-if="visitors.length === 0">
          <td colspan="6" class="text-center">Tidak ada data tersedia</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
useHead({
  title: "PRESENSI",
  meta: [
    {
      name: "description",
      content: "Halaman REKAPAN PER HARI",
    },
  ],
});
import { ref, onMounted, watch } from 'vue';
const supabase = useSupabaseClient();
const keyword = ref("");
const visitors = ref([]);
const total = ref(0);
const loading = ref(false);

// Debounce function to limit API calls
const debounce = (func, delay) => {
  let timeout;
  return (...args) => {
    clearTimeout(timeout);
    timeout = setTimeout(() => func.apply(this, args), delay);
  };
};

const getpengunjung = async () => {
  loading.value = true; // Set loading to true
  const { data, error } = await supabase
    .from("Presensi")
    .select(`*, siswa(*), keterangan(*), hari(*), Opsi(*)`)
    .ilike("siswa.nama", `%${keyword.value}%`)
    .order('id', { ascending: false });

  loading.value = false; // Set loading to false
  if (error) {
    console.error('Error fetching visitors:', error);
  } else {
    visitors.value = data || []; // Ensure visitors is always an array
  }
};

// Create a debounced version of the getpengunjung function
const debouncedGetPengunjung = debounce(getpengunjung, 300);

// Watch for changes in the keyword and call the debounced function
watch(keyword, () => {
  debouncedGetPengunjung();
});

// Fetch data when component is mounted
onMounted(() => {
  getpengunjung();
});
</script>

<style scoped>
.table {
  margin-top: 20px;
}
</style>
