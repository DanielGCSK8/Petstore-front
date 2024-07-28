<template>
    <div class="p-5">
        <div v-if="loading == false" class="table-responsive">
            <table class="table align-middle">
                <thead>
                    <tr>
                        <th scope="col">ID</th>
                        <th scope="col">Categoría</th>
                        <th scope="col">Nombre</th>
                        <th scope="col">Urls fotos</th>
                        <th scope="col">Tags</th>
                        <th scope="col">Estados</th>
                        <th scope="col">Acciones</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="pet in pets" :key="pet.id" class="align-bottom">
                        <td><span class="fw-bold">{{ pet.id }}</span></td>
                        <td><span class="fw-bold">{{ pet.category.name }}</span></td>
                        <td><span class="fw-bold">{{ pet.name }}</span></td>
                        <td>
                            <div v-for="(url, index) in JSON.parse(pet.photoUrls)" :key="index" style="width: 400px;">
                                <a type="button" @click.prevent="handleLinkClick(url)" class="link-style">{{ url }}</a>
                            </div>
                        </td>
                        <td><span class="fw-bold">{{ pet.tags.name }}</span></td>
                        <td> <span :class="statusBadgeClass(pet.status)" class="badge">
                                {{ pet.status }}
                            </span></td>
                        <td><nuxt-link :to="`/pets/${pet.id}`"><i class="fa-solid fa-eye"></i></nuxt-link>
                            <a type="button" @click="editPet(pet)"><i class="fa-solid fa-edit ms-2"></i></a>
                            <a type="button" @click="confirmDelete(pet.id)"><i class="fa-solid fa-trash ms-2"
                                    style="color: red"></i></a>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>
        <div class="d-flex justify-content-center">
            <div v-if="loading == true" class="spinner-border text-primary" role="status">
                <span class="visually-hidden">Loading...</span>
            </div>
        </div>
        <div v-if="loading == false" className='mt-5'>
            <button @click="createPet" class="btn btn-success"><span className='fw-bold'>Crear Pet</span></button>
        </div>
        <PetForm :showModal.sync="showModal" :statusOptions="statusOptions" @pet-saved="fetchPets" :category="category"
            :tags="tags" :pet="selectedPet" />
    </div>
</template>
  
<script>
import axios from 'axios';
import { BButton } from 'bootstrap-vue';
import PetForm from '~/components/PetForm.vue';


export default {
    components: {
        PetForm,
        BButton
    },
    data() {
        return {
            loading: true,
            showModal: false,
            pets: [],
            category: [],
            tags: [],
            statusOptions: [
                { text: 'Available', value: 'available' },
                { text: 'Pending', value: 'pending' },
                { text: 'Sold', value: 'sold' }
            ],
            category_tags: [

            ],
            selectedPet: null
        };
    },
    methods: {
        async fetchPets() {
            try {
                const response = await axios.get('http://127.0.0.1:8000/api/pet/');
                this.pets = response.data.pets;
                this.category = response.data.category;
                this.tags = response.data.tags;
                this.loading = false;
            } catch (error) {
                console.error('Error fetching pets:', error);
            }
        },
        statusBadgeClass(status) {
            switch (status) {
                case 'available':
                    return 'bg-success';
                case 'pending':
                    return 'bg-warning';
                case 'sold':
                    return 'bg-danger';
                default:
                    return 'bg-secondary';
            }
        },
        editPet(pet) {
            this.selectedPet = pet;
            this.showModal = true;
        },
        createPet() {
            this.selectedPet = null;
            this.showModal = true;
        },
        async deletePet(id) {
            try {
                await axios.delete(`http://127.0.0.1:8000/api/pet/${id}`, {
                    headers: {
                        'api_key': 'abc1234'
                    }
                });
                return true;
            } catch (error) {
                console.error('Error al eliminar el madero:', error);
                return false;
            }
        },
        async confirmDelete(id) {
            const result = await this.$swal.fire({
                title: '¿Está seguro que desea eliminar el pet?',
                icon: 'warning',
                showCancelButton: true,
                confirmButtonColor: '#3085d6',
                cancelButtonColor: '#d33',
                confirmButtonText: 'Sí',
                cancelButtonText: 'Cancelar'
            });

            if (result.isConfirmed) {
                const success = await this.deletePet(id);
                if (success) {
                    await this.fetchPets();
                    this.$swal.fire({
                        title: '¡Eliminado!',
                        text: 'Pet eliminado exitosamente.',
                        icon: 'success'
                    });
                } else {
                    this.$swal.fire({
                        title: 'Error',
                        text: 'Hubo un problema al eliminar el pet.',
                        icon: 'error'
                    });
                }
            }
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
    },
    mounted() {
        this.fetchPets();
    }
}
</script>

<style scoped>
.link-style {
  color: #007bff;
  text-decoration: underline;
}

.link-style:hover {
  color: #0056b3;
}
</style>