<template>
  <div>
    <NavComponent />
    <PetsComponent />
  </div>
</template>

<script>
import NavComponent from '~/components/Nav.vue';
import PetsComponent from '~/components/Pets.vue';

export default {
  name: 'IndexPage',
  components: {
    NavComponent,
    PetsComponent
  }
}
</script>
