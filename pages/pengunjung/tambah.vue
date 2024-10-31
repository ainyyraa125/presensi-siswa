<template>
  <div class="container-fluid p-5">
    <div class="row">
      <div class="col-lg-12">
        <h2 class="text-center my-4">ISI DATA SISWA</h2>
        <form @submit.prevent="kirimData">
          <div class="mb-3">
            <select
              v-model="form.siswa"
              class="form-control form-control-lg form-select radius"
            >
              <option value="">Nama</option>
              <option
                v-for="(member, i) in members"
                :key="i"
                :value="member.id"
              >
                {{ member.nama }}
              </option>
              <!-- <option value="Siswa">Siswa</option>
              <option value="Guru">Guru</option>
              <option value="Staf">Staf</option>
              <option value="Umum">Umum</option> -->
            </select>
          </div>
          <div class="mb-3">
            <select
              v-model="form.hari"
              class="form-control form-control-lg form-select radius"
            >
              <option value="">Hari</option>
              <option v-for="(obje, i) in objec" :key="i" :value="obje.id">
                {{ obje.hari }}
              </option>
            </select>
          </div>
          <div class="mb-3">
            <select
              v-model="form.keterangan"
              class="form-control form-control-lg form-select radius"
            >
              <option value="">Kehadiran</option>
              <option v-for="(item, i) in objectives" :key="i" :value="item.id">
                {{ item.keterangan }}
              </option>
            </select>
          </div>
          <div class="mb-3">
            <select
              v-model="form.opsi"
              class="form-control form-control-lg form-select radius"
            >
              <option value="">Opsi</option>
              <option v-for="(op, i) in ops" :key="i" :value="op.id">
                {{ op.opsi }}
              </option>
            </select>
          </div>
          <div class="tombol">
            <button
              type="submit"
              class="btn btn-lg btn-primary radius"
              formaction=""
            >
              KIRIM
            </button>
            <nuxt-link to="/">
            <button type="submit"
                class="btn btn-lg btn-secondary radius " style="float: right;">
                KEMBALI
              </button>
            </nuxt-link>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>
 <style scoped>
.radius {
  border-radius: 20px;
}
 input {
  border: 1px solid #000;
  height: 42px;
}
 select {
  border: 1px solid #000;
}
 button {
  border: 1px solid #000;
  color: #fff;
}
 .kembali {
  position: fixed;
  bottom: 30px;
  right: 30px;
  border-radius: 20px;
}
</style>
<script setup>
useHead({
  title: "PRESENSI",
  meta: [
    {
      name: "description",
      content: "Halaman ISI DATA",
    },
  ],
});
const supabase = useSupabaseClient();
const members = ref([]);
const objectives = ref([]);
const objec = ref([]);
const ops = ref([]);
const form = ref({
  siswa: "",
  hari: "",
  keterangan: "",
  opsi: "",
});
 const kirimData = async () => {
  // console.log(form.value)
  const { error } = await supabase.from("Presensi").insert([form.value]);
  if (!error) navigateTo("/pengunjung");
};
 const getNama = async () => {
  const { data, error } = await supabase.from("Siswa").select("*");
  if (data) members.value = data;
};
const getKehadiran = async () => {
 const { data, error } = await supabase.from("Statuskehadiran").select("*");
 if (data) objectives.value = data;
};
const gethari = async () => {
 const { data, error } = await supabase.from("statushari").select("*");
 if (data) objec.value = data;
};
const getOpsi = async () => {
 const { data, error } = await supabase.from("Opsi").select("*");
 if (data) ops.value = data;
};

onMounted(() => {
  getNama();
  gethari();
  getKehadiran();
  getOpsi();

});
</script>
