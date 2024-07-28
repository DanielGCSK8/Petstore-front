<template>
    <b-modal v-model="showModal" title="Crear Pet" @hide="resetForm" hide-footer hide-header>
        <div class="d-flex justify-content-end">
            <a type="button" @click="closeModal"><i class="fa-solid fa-xmark"></i></a>
        </div>
        <b-form @submit.prevent="handleSubmit">
            <div class="form-group">
                <label for="category">Categoría</label>
                <select v-model="form.category" class="form-control mt-2" id="category" required>
                    <option value="">Seleccione</option>
                    <option v-for="categories in category" :key="categories.id" :value="categories.id">
                        {{ categories.name }}
                    </option>
                </select>
            </div>

            <b-form-group class="mt-2" label="Nombre" label-for="name-input">
                <b-form-input id="name-input" class="mt-2" v-model="form.name" required
                    placeholder="Ingresa nombre"></b-form-input>
            </b-form-group>

            <div class="form-group">
                <label for="photoUrls" class="mt-2">Url de las fotos</label>
                <textarea v-model="form.photoUrls" class="form-control" id="photoUrls"
                    placeholder="Inserta URLs separadas por comas" rows="3"></textarea>
            </div>

            <div class="form-group">
                <label for="tags" class="mt-2">Tags</label>
                <select v-model="form.tags" class="form-control mt-2" required>
                    <option value="">Seleccione</option>
                    <option v-for="tag in tags" :key="tag.id" :value="tag.id">
                        {{ tag.name }}
                    </option>
                </select>
            </div>
            <b-form-group label="Estado" class="mt-2" label-for="status-input">
                <b-form-select id="status-input" class="form-control mt-2" v-model="form.status" :options="statusOptions"
                    required></b-form-select>
            </b-form-group>

            <div class="mt-4 d-flex justify-content-end">
                <b-button v-if="!pet" type="submit" variant="primary">Guardar</b-button>
                <b-button v-if="pet" type="submit" variant="primary">Actualizar</b-button>
            </div>
        </b-form>
    </b-modal>
</template>
  
<script>
import { BButton, BForm, BFormGroup, BFormInput, BFormSelect, BModal } from 'bootstrap-vue';
import axios from 'axios';

export default {
    components: {
        BButton,
        BForm,
        BFormGroup,
        BFormInput,
        BFormSelect,
        BModal
    },
    props: {
        showModal: {
            type: Boolean,
            required: true
        },
        category: {
            type: Array,
            required: true
        },
        tags: {
            type: Array,
            required: true
        },
        statusOptions: {
            type: Array,
            required: true
        },
        pet: {
            type: Object,
            default: null
        }
    },
    data() {
        return {
            localShowModal: this.showModal,
            form: {
                category: '',
                name: '',
                photoUrls: '',
                tags: '',
                status: 'available'
            }
        };
    },
    watch: {
        showModal(newVal) {
            this.localShowModal = newVal;
            if (newVal && this.pet) {
                this.form = {
                    category: this.pet.category.id,
                    name: this.pet.name,
                    photoUrls: JSON.parse(this.pet.photoUrls),
                    tags: this.pet.tags.id,
                    status: this.pet.status
                };
            }
        },
        localShowModal(newVal) {
            this.$emit('update:showModal', newVal);
        }
    },
    methods: {
        showAlertSuccessForm(error) {
            if (error == undefined) {
                if (this.pet) {
                    this.$swal.fire({
                        text: 'Pet actualizado exitosamente',
                        icon: 'success'
                    });
                } else {
                    this.$swal.fire({
                        text: 'Pet guardado exitosamente',
                        icon: 'success'
                    });
                }

            } else {
                if (this.pet) {
                    this.$swal.fire({
                        title: "Error",
                        text: "Hubo un problema al actualizar el pet.",
                        icon: "error"
                    });
                } else {
                    this.$swal.fire({
                        title: "Error",
                        text: "Hubo un problema al guardar el pet.",
                        icon: "error"
                    });
                }
            }
        },
        closeModal() {
            this.$emit('update:showModal', false);
        },
        async handleSubmit() {
            let alert;
            try {
                if (typeof this.form.photoUrls === 'string') {
                    this.form.photoUrls = this.form.photoUrls.split(',').map(url => url.trim());
                }

                if (this.pet) {
                    await axios.post(`http://127.0.0.1:8000/api/pet/${this.pet.id}`, this.form);
                } else {
                    await axios.post('http://127.0.0.1:8000/api/pet', this.form);
                }
                this.localShowModal = false;
                this.$emit('pet-saved');
                this.resetForm();
                this.showAlertSuccessForm(alert);
            } catch (error) {
                alert = error;
                this.showAlertSuccessForm(alert);
                console.error('Error submitting form:', error);
            }
        },
        resetForm() {
            this.form = {
                name: '',
                category: '',
                photoUrls: '',
                tags: '',
                status: 'available'
            };
        }
    }
}
</script>

