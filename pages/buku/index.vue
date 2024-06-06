import { NuxtLink } from '#build/components';
<template>
  <div class="container-fluid">
    <div class="row"></div>
    <div class="col-lg-12">
      <h2 class="text-center my-4">BUKU</h2>
      <div class="my-3">
        <form @submit.prevent="getBuku">
        <input v-model="keyword" type="search" class="form-control rounded-5" placeholder="Mau Baca apa hari ini?">
      </form>
      </div>
      <div class="my-3 text-muted">menampilkan {{ buku?.length }} dari {{ totalBuku }}</div>
      <div class="row justify-content-evenly">
        <div v-for="(buku,i) in buku" :key="i" class="col-lg-2">
          <NuxtLink :to="`/buku/${buku.id}`">
            <div class="card mb-3">
              <div class="card-body">
              <img :src="buku.cover" class="cover" :alt="buku.judul">
            </div>
          </div>
          </NuxtLink>
        </div>
    </div>
    </div>
    <NuxtLink to="/">
      <button type="submit" class="btn btn-lg rounded-5">Kembali</button>
    </NuxtLink>
  </div>
</template>

<script setup>
const supabase = useSupabaseClient();

const buku = ref([]);
const totalBuku = ref(0);

const getBuku = async () => {
  const { data, error } = await supabase.from("buku").select("*, kategori(*)").ilike("judul", `%${keyword.value}%`);
  if(data) buku.value = data;
}
const getTotalBuku = async () => {
  const { count, error } = await supabase.from("buku").select("*, kategori(*)", { count: "exact", head: true});
  if(count) totalBuku.value = count;
}

onMounted(() => {
  getBuku();
  getTotalBuku();
});
const keyword = ref("");
</script>

<style scoped>
.card-body {
  width: 100%;
  height: 500px;
  padding: 0;
}
.cover {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: 0 30;
}
.btn{
  background-color: grey;
}
</style>

