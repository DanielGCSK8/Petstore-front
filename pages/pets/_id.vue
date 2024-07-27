<template>
  <div>
    <NavComponent />

    <div class="p-5" v-if="pet">
      <!-- <h1>{{ pet.name }}</h1>
      <p><strong>Categoría:</strong> {{ pet.category.name }}</p>
      <p><strong>Url de las fotos:</strong></p>
      <ul>
        <li v-for="(url, index) in pet.photoUrls" :key="index">
          <a :href="url" target="_blank">{{ url }}</a>
        </li>
      </ul>
      <p><strong>Tags:</strong></p>
      <ul>
        <li>
          {{ pet.tags.name }}
        </li>
      </ul>
      <p><strong>Estado:</strong> {{ pet.status }}</p> -->
      <div class="d-flex justify-content-center">
        <div class="card border-info" style="width: 65vw;">
          <h5 class="card-header">Pet</h5>
          <div class="card-body">
            <div class="d-flex justify-content-center">
              <p class="fs-5 fw-bold">Nombre:</p>
              <p class="fs-5 ms-2">{{ pet.name }}</p>
            </div>
            <div class="d-flex justify-content-center">
              <p class="fs-5 fw-bold">Categoría:</p>
              <p class="fs-5 ms-2">{{ pet.category.name }}</p>
            </div>
            <div>
              <div class="d-flex justify-content-center">
                <p class="fs-5 fw-bold">Url de las fotos:</p>

              </div>
              <div class="d-flex justify-content-center" v-for="(url, index) in JSON.parse(pet.photoUrls)" :key="index">
                <a :href="url" target="_blank">
                  <p class="fs-6">{{ url }}</p>
                </a>
              </div>
            </div>
            <div class="d-flex justify-content-center">
              <p class="fs-5 fw-bold">Tags:</p>
              <p class="fs-5 ms-2">{{ pet.tags.name }}</p>
            </div>
            <div class="d-flex justify-content-center">
              <p class="fs-5 fw-bold">Estado:</p>
              <p class="fs-5 ms-2">{{ pet.status }}</p>
            </div>
            <div>
              <nuxt-link :to="'/'" class="btn btn-primary">Volver</nuxt-link>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="p-5" v-else>
      <div class="d-flex justify-content-center">
        <p>Loading...</p>
      </div>
    </div>
  </div>
</template>
  
<script>
import axios from 'axios';
import NavComponent from '~/components/Nav.vue';

export default {
  components: {
    NavComponent
  },
  async asyncData({ params }) {
    console.log(params)
    try {
      const response = await axios.get(`http://127.0.0.1:8000/api/pet/${params.id}`);
      return { pet: response.data.data };
    } catch (error) {
      console.error('Error fetching pet:', error);
      return { pet: null };
    }
  }
}
</script>