<script>
import 'bootstrap/dist/css/bootstrap.min.css';
import 'bootstrap/dist/js/bootstrap.bundle.min.js';
import axios from "axios";
export default {
  data() {

    return {
      apartments: [],
      searchQuery: "",
      searchApartment: null,
    };
  },
  computed: {
    filteredApartments() {
      if (!this.searchQuery) {
        return this.apartments;
      }
      return this.apartments.filter((apartment) => {
        return apartment.name
          .toLowerCase()
          .includes(this.searchQuery.toLowerCase());
      });
    },
  },
  methods: {
    loadData() {
      axios.get("http://127.0.0.1:8000/api/apartments").then((resp) => {
        this.apartments = resp.data;
      });
    },
    getImgUrl(apartment) {
      return `http://127.0.0.1:8000/storage/${apartment.images}`;
    },
  },
  mounted() {
    this.loadData();
  },
};
</script>

<template>
  <div class="gallery">
    <div class="row d-flex justify-content-center p-0">
      <div class="col-xl-2 col-lg-3 col-md-4 col-sm-6 col-xs-8 m-2 p-2 rounded-3" v-for="apartment in apartments"
        :key="apartment.id">

        <!-- <div :id="'carouselExampleIndicators' + apartment.id" class="carousel slide">
          <div class="carousel-indicators">
            <button type="button" data-bs-target="#carouselExampleIndicators" data-bs-slide-to="0" class="active"
              aria-current="true" aria-label="Slide 1"></button>
            <button type="button" data-bs-target="#carouselExampleIndicators" data-bs-slide-to="1"
              aria-label="Slide 2"></button>
            <button type="button" data-bs-target="#carouselExampleIndicators" data-bs-slide-to="2"
              aria-label="Slide 3"></button>
          </div>
          <div class="carousel-inner">
            <div class="card-image p-0 carousel-item active" :class="{ active: index === 0 }">
              <img :src="getImgUrl(apartment)" class="card-img-top rounded-3 d-block img-fluid w-100 h-100"
                style="max-height: 300px; min-height: 220px; object-fit: cover;" alt="" />
            </div>
          </div>
          <button class="carousel-control-prev" type="button" data-bs-target="#carouselExampleIndicators"
            data-bs-slide="prev">
            <span aria-hidden="true"><i class="fa-solid fa-circle-arrow-left"></i></span>
          </button>
          <button class="carousel-control-next" type="button" data-bs-target="#carouselExampleIndicators"
            data-bs-slide="next">
            <span aria-hidden="true"><i class="fa-solid fa-circle-arrow-right"></i></span>
          </button>
        </div> -->

        <div class="carousel-inner rounded-3">
          <div class="card-image d-block img-fluid carousel-item active" :class="{ active: index === 0 }">
            <img :src="getImgUrl(apartment)" class="card-img-top rounded-3 d-block img-fluid w-100 h-100"
              style="max-height: 300px; min-height: 220px; object-fit: cover;" alt="" />
          </div>
        </div>

        <div class="card-body h-50">
          <div class="row d-flex">
            <h2></h2>
            <span class="text-decoration-none"><router-link
                :to="{ name: 'apartments.show', params: { id: apartment.id } }">{{ apartment.name }}</router-link></span>
            <span class="text-decoration-none">{{ apartment.address }}</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped lang="scss">
.gallery {
  color: #001632;
}

.gallery a {
  text-decoration: none;
  color: #001632;
}

.row {
  width: 100%;
}

.carousel-inner {
  box-shadow: 0 0 15px 0 rgba(#000, .1), 0 5px 10px 0 rgba(#000, .1);
}

// STILI CAROSELLO
/*
.carousel-indicators button {
  width: 6px;
  height: 6px;
  border-radius: 50%;
  margin: 0 5px;
  background-color: #888;
  // Colore di sfondo dei pallini non attivi 
}

// Nascondi le frecce direzionali all'inizio 
.carousel-control-prev,
.carousel-control-next {
  opacity: 0;
  transition: opacity 0.3s ease;
}

// Mostra le frecce direzionali quando il mouse è sopra il carosello

.carousel:hover .carousel-control-prev,
.carousel:hover .carousel-control-next {
  opacity: .8;
}

// Stile per le frecce direzionali

.carousel-control-prev,
.carousel-control-next {
  font-size: 1rem;
  // Regola la dimensione delle frecce
  color: #fff;
  // Colore delle frecce
  background: transparent;
  // Sfondo trasparente
  border: none;
  // Senza bordo
  outline: none;
  // Senza contorno
}

// percorsi per inserire lo stile scss nel partials
// @use "../../scss/partials/mixins" as *;
// @use "../../scss/partials/variables" as *; 
*/

</style>
