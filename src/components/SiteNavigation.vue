<template>
  <header class="sticky top-0 bg-weather-primary shadow-lg">
    <nav class="container flex flex-col sm:flex-row items-center gap-4 text-white py-6">
      <RouterLink :to="{ name: 'home' }">
        <div class="flex items-center gap-3">
          <i class="fa-solid fa-sun text-2xl"></i>
          <p class="text-2xl">
            អាកាសធាតុក្នុងស្រុក
          </p>
        </div>
      </RouterLink>

      <div class="flex gap-3 flex-1 justify-end">
        <i class="fa-solid fa-circle-info text-xl hover:text-weather-secondary duration-150 cursor-pointer"
          @click="toggleModal"></i>
        <i class="fa-solid fa-plus text-xl hover:text-weather-secondary duration-150 cursor-pointer" @click="addCity"
          v-if="route.query"></i>
      </div>

      <BaseModal :modalActive="modalActive" @close-modal="toggleModal">
        <div class="text-black">
          <h1 class="text-2xl mb-1">
            អំពី:</h1>
          <p class="mb-4">
            អាកាសធាតុក្នុងស្រុកអនុញ្ញាតឱ្យអ្នកតាមដានបច្ចុប្បន្ននិង
            អាកាសធាតុនាពេលអនាគតនៃទីក្រុងដែលអ្នកជ្រើសរើស។
          </p>
          <h2 class="text-2xl">របៀបដែលវាដំណើរការ:</h2>
          <ol class="list-decimal list-inside mb-4">
            <li>
              ស្វែងរកទីក្រុងរបស់អ្នកដោយបញ្ចូលឈ្មោះទៅក្នុង
              របារស្វែងរក។
            </li>
            <li>
              ជ្រើសរើសទីក្រុងមួយនៅក្នុងលទ្ធផល វានឹងកើតឡើង
              អ្នកទៅកាន់អាកាសធាតុបច្ចុប្បន្នសម្រាប់ការជ្រើសរើសរបស់អ្នក។
            </li>
            <li>
              តាមដានទីក្រុងដោយចុចលើរូបតំណាង "+" នៅក្នុងផ្ទាំង
              ខាងលើស្តាំ។ វានឹងរក្សាទុកទីក្រុងដើម្បីមើលនៅ a
              ពេលក្រោយនៅលើទំព័រដើម។
            </li>
          </ol>

          <h2 class="text-2xl">
            ការដកទីក្រុងចេញ</h2>
          <p>
            ប្រសិនបើអ្នកលែងចង់តាមដានទីក្រុងទៀតហើយ សូមជ្រើសរើស
            ទីក្រុងនៅក្នុងទំព័រដើម។ នៅផ្នែកខាងក្រោមនៃ
            ទំព័រនឹងមានជម្រើសដើម្បីលុបទីក្រុង។
          </p>
        </div>
      </BaseModal>
    </nav>
  </header>
</template>

<script setup>
import { RouterLink, useRoute, useRouter } from "vue-router";
import { uid } from "uid";
import { ref } from "vue";
import BaseModal from "./BaseModal.vue";

const savedCities = ref([]);
const route = useRoute();
const router = useRouter();
const addCity = () => {
  if (localStorage.getItem("savedCities")) {
    savedCities.value = JSON.parse(
      localStorage.getItem("savedCities")
    );
  }

  const locationObj = {
    id: uid(),
    state: route.params.state,
    city: route.params.city,
    coords: {
      lat: route.query.lat,
      lng: route.query.lng,
    },
  };

  savedCities.value.push(locationObj);
  localStorage.setItem(
    "savedCities",
    JSON.stringify(savedCities.value)
  );

  let query = Object.assign({}, route.query);
  delete query.preview;
  query.id = locationObj.id;
  router.replace({ query });
};

const modalActive = ref(null);
const toggleModal = () => {
  modalActive.value = !modalActive.value;
};
</script>
