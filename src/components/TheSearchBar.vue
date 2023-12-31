<script>
import axios from "axios";
import AdvancedSearchCard from "./AdvanceSearchCard.vue";

export default {
  data() {
    return {
      searchQuery: "",
      showAdvancedSearchCard: false,
      searchResults: [],
      advancedFilters: {
        location: "",
        distance: 0,
        services: [],
      },
      availableServices: [],
    };
  },
  methods: {
    getApartments() {
      axios
        .get("http://127.0.0.1:8000/api/apartments")
        .then((res) => {
          this.searchResults = res.data;
        })
        .catch((error) => {
          console.error("Errore durante la richiesta API:", error);
        });
    },
    async searchApartments() {
      await this.fetchServices();

      const apiUrl = "http://127.0.0.1:8000/api/apartments";

      axios
        .get(apiUrl, {
          params: { query: this.searchQuery, ...this.advancedFilters },
        })
        .then((response) => {
          this.searchResults = response.data;
        })
        .catch((error) => {
          console.error(error);
        });
    },

    openAdvancedSearch() {
      this.showAdvancedSearchCard = true;
      document.body.classList.add("advanced-search-open");
      document.addEventListener("click", this.closeAdvancedSearch);
    },
    closeAdvancedSearch(event) {
      const searchContainer = this.$refs.searchContainer;
      if (
        searchContainer &&
        !searchContainer.contains(event.target) &&
        !event.target.classList.contains("advanced-search-card-trigger")
      ) {
        this.showAdvancedSearchCard = false;
        document.removeEventListener("click", this.closeAdvancedSearch);
        document.body.classList.remove("advanced-search-open");
      }
    },
    applyFilters(filters) {
      this.advancedFilters = { ...this.advancedFilters, ...filters };
    },
  },
  components: {
    AdvancedSearchCard,
  },
};
</script>

<template>
  <div
    class="container d-flex align-items-center flex-grow-1 justify-content-end"
  >
    <div
      class="search-bar-container"
      :class="{ 'advanced-search-open': showAdvancedSearchCard }"
    >
      <div class="search-box">
        <button class="btn-search" @click.prevent="searchApartments">
          <i class="fas fa-search"></i>
        </button>
        <input
          v-model="searchQuery"
          @focus="openAdvancedSearch"
          class="input-search"
          type="text"
          placeholder="Dove si va ?"
        />
      </div>

      <!-- Search Results -->
      <div class="page-content">
        <div class="card-deck">
          <div
            v-for="apartment in searchResults"
            :key="apartment.id"
            class="card"
          >
            <div class="card-body">
              <h5 class="card-title">{{ apartment.name }}</h5>
            </div>
          </div>
        </div>
      </div>

      <!-- Advanced Search Card -->
      <div
        ref="advancedSearchCard"
        class="advanced-search-card"
        v-if="showAdvancedSearchCard"
      >
        <div @click.stop>
          <AdvancedSearchCard @applyFilters="applyFilters" />
        </div>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.search-box {
  position: relative;
}

.search-icon {
  position: absolute;
  top: 50%;
  left: 15px;
  transform: translateY(-50%);
}

.advanced-search-card {
  position: fixed;
  top: 4.6rem;
  left: 50%;
  transform: translateX(-50%);
  width: 100%;
  height: 30%;
  padding-left: 3rem;
  background-color: #f8f9fa;
}

.search-icon {
  position: absolute;
  top: 50%;
  left: 15px;
  transform: translateY(-50%);
}

.search-box {
  width: fit-content;
  height: fit-content;
  position: relative;
  margin-bottom: 20px;
}

.input-search {
  height: 50px;
  width: 50px;
  border-style: none;
  padding: 10px;
  font-size: 18px;
  letter-spacing: 2px;
  outline: none;
  border-radius: 25px;
  transition: all 0.5s ease-in-out;
  background-color: #dbdbdb;
  padding-right: 40px;
  color: #d87767;
}

.input-search::placeholder {
  color: #001632;
  font-size: 18px;
  letter-spacing: 2px;
  font-weight: bold;
}

.btn-search {
  width: 50px;
  height: 50px;
  border-style: none;
  font-size: 20px;
  font-weight: bold;
  outline: none;
  cursor: pointer;
  border-radius: 50%;
  position: absolute;
  right: 0px;
  color: #001632;
  background-color: transparent;
  pointer-events: painted;
}

.btn-search:focus ~ .input-search,
.input-search:focus {
  width: 300px;
  border-radius: 0px;
  background-color: transparent;
  border-bottom: 1px solid rgba(255, 255, 255, 0.5);
  transition: all 500ms cubic-bezier(0, 0.11, 0.35, 2);
}
</style>
