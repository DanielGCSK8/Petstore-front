<template>
     <div>
            <nav class="navbar navbar-light bg-light">
                <div class="container-fluid">
                    <div class="navbar-brand" to="/">
                        <img :src="logo" height="55px" />
                    </div>
                        <!-- <ul className="navbar-nav me-auto mb-2 mb-lg-0">
                            <li className="nav-item">
                                <Link className="nav-link fw-bold" to="/adminMaderos">Administrar Maderos</Link>
                            </li>
                        </ul> -->

                </div>
            </nav>
        </div>
</template>

<script>
import logo from '~/assets/images/petstore.jpg';
export default {
    data() {
      return {
        logo
      };
    }
  }
 </script>