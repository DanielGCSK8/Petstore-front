<template>
     <div>
            <nav class="navbar navbar-light bg-light">
                <div class="container-fluid">
                    <div class="navbar-brand" to="/">
                        <div class="d-flex">
                            <img :src="logo" height="75px" />
                            <p class="mt-3 fst-italic">PetStore</p>
                        </div>
                    </div>
                </div>
            </nav>
        </div>
</template>

<script>
import logo from '~/assets/images/petstore.png';
export default {
    data() {
      return {
        logo
      };
    }
  }
 </script>