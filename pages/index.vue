<template>
  <div class="">
    <div class="w-1/2 m-auto mt-10 bg-rose-300 rounded-md h-72">
      <h3 class="text-2xl text-center font-bold text-gray-800 pt-5">
        Rika TodoList
      </h3>
      <form @submit.prevent="save()">
        <div class="flex items-center">
          <input
            type="text"
            class="py-2 w-[80%] m-auto rounded-full bg-gray-100 border border-gray-400 mt-5"
            v-model="isi"
          />
          <button
            type="submit"
            class="py-2 px-5 bg-sky-500 rounded-md text-white text-center mt-5 font-bold mx-3"
          >
            tambah
          </button>
        </div>
      </form>
      <div class="w-full px-10" v-for="isi in datas" :key="isi.id">
        <div
          class="w-full bg-gray-100 text-gray-800 font-semibold h-10 rounded-md mt-3 flex flex-wrap align-middle"
        >
          <button @click="deleteUser(isi)" class="mx-3">👌</button>
          <h3
            class="text-gray-800 font-semibold mx-5 my-auto items-center align-middle text-xl"
          >
            {{ isi.isi }}
          </h3>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
const supabase = useSupabaseClient();
const isi = ref([]);
const datas = ref();
const router = useRouter();

async function save() {
  await supabase.from("list").insert({
    isi: isi.value,
  });

  getdata();
}
async function getdata() {
  const { data, error } = await supabase.from("list").select();
  datas.value = data;
}
async function deleteUser(isi) {
  const { error, status } = await supabase
    .from("list")
    .delete()
    .eq("id", isi.id);

  getdata();
}
onMounted(() => {
  getdata();
});
</script>
