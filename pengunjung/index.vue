<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-lg-12">
        <h2 class="text-center my-4">RIWAYAT KUNJUNGAN</h2>
        <div class="my-3">
          <form @submit.prevent="getBuku">
            <input v-model="keyword" type="search" class="form-control form-control-lg rounded-5" placeholder="Filter...">
          </form>
        </div>
        <div class="my-3 text-muted">menampilkan daftar pengunjung</div>
        <table class="table table-bordered">
          <thead>
            <tr>
              <td>#</td>
              <td>NAMA</td>
              <td>KEANGGOTAAN</td>
              <td>WAKTU</td>
              <td>KEPERLUAN</td>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(visitors,i) in visitors" :key="i">
              <td>{{ i+i }}.</td>
              <td>{{ visitors.nama }}.</td>
              <td>{{ visitors.keanggotaan.nama }}.</td>
              <td>{{ visitors.tanggal }}.</td>
              <td>{{ visitors.keperluan.nama }}.</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
    <NuxtLink to="/pengunjung/tambah">
      <button type="submit" class="btn btn-primary rounded-5 px-5">Kembali</button>
    </NuxtLink>
  </div>
</template>

<script setup>
const supabase = useSupabaseClient()
const keyword = ref('')
const visitors = ref ([])

const getPengunjung = async () => {
  const { data, error } = await supabase.from('pengunjung').select(`*, keanggotaan(*), keperluan(*)`)
  if(data) visitors.value = data
}
const getBuku = async () => {
  const { data, error } = await supabase.from('pengunjung').select(`*, keanggotaan(*), keperluan(*)`)
  .ilike('nama', `%${keyword.value}%`)
  if(data) visitors.value = data
}

onMounted(() => {
  getPengunjung()
  getBuku()
})
</script>

<style scoped>
.btn{
  background-color: rgb(120, 126, 115);
}
.form-control{
  background-color: rgb(120, 126, 115);
}
</style>