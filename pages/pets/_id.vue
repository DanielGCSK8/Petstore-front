<template>
  <div>
    <NavComponent />

    <div class="p-5" v-if="pet">
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
                <div v-if="loading" class="spinner-border text-primary mt-2" role="status">
                  <span class="visually-hidden">Loading...</span>
                </div>
                <a class="mt-3" @click.prevent="handleLinkClick(url)">
                  <img :src="url" height="80px" @load="handleImageLoad" @error="handleImageError($event)"
                    v-show="!loading" />
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
        <div class="spinner-border text-primary mt-2" role="status">
          <span class="visually-hidden">Loading...</span>
        </div>
      </div>
    </div>
  </div>
</template>
  
<script>
import axios from 'axios';
import NavComponent from '~/components/Nav.vue';

export default {
  data() {
    return {
      loading: true
    };
  },
  components: {
    NavComponent
  },
  async asyncData({ params }) {
    try {
      const response = await axios.get(`http://127.0.0.1:8000/api/pet/${params.id}`);
      return { pet: response.data.data };
    } catch (error) {
      console.error('Error fetching pet:', error);
      return { pet: null };
    }
  },
  methods: {
    handleImageError(event) {
      this.loading = false;
      event.target.src = 'https://www.puroverso.uy/images/virtuemart/product/9789974719019.jpg';
    },
    handleLinkClick(url) {
      // Valida si la URL es correcta antes de redirigir
      if (this.isValidUrl(url)) {
        window.open(url, '_blank');
      } else {
        console.error('Invalid URL:', url);
      }
    },
    isValidUrl(url) {
      return url.startsWith('http');
    },
    handleImageLoad() {
      this.loading = false;
    }
  }
}
</script>