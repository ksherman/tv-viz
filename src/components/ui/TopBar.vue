<template>
  <div class="topbar-wrapper">
    <div class="topbar-search">
      <input
        type="text"
        name="search-query"
        placeholder="search for a show..."
        @keyup.enter="handleSearchQuery"
        v-model="searchQuery" />
      <div
        class="search-query-results"
        :class="{ visible: searchQuery && (getSearchError || getSearchResults) }"
      >
        <template v-if="getSearchError">
          <div class="no-result">{{ getSearchError }}</div>
        </template>
        <template v-else-if="getSearchResults">
          <div class="result" v-for="show in getSearchResults.Search" :key="`show-${show.imdbID}`">
            <div class="result-image">
              <img
                v-if="show.Poster !== 'N/A'"
                :src="show.Poster"
              />
            </div>
            <div class="result-details">
              <div class="result-title">{{ show.Title }}</div>
              <div class="result-year">{{ show.Year }}</div>
              <div class="result-view">
                <button class="button" @click="handleShowChange(show.imdbID)">View Show</button>
              </div>
            </div>
          </div>
        </template>
        <div class="results-actions text-right">
          <button class="button wide" @click="clearSearchQuery">close</button>
        </div>
      </div>
    </div>
    <div class="topbar-title">
      <router-link tag="h1" :to="{ name: 'main' }">TV_Viz</router-link>
    </div>
  </div>
</template>

<script>
import { mapActions, mapGetters } from 'vuex';

export default {
  name: 'TopBar',
  data() {
    return {
      searchQuery: '',
    };
  },
  computed: {
    ...mapGetters(['getSearchError', 'getSearchResults']),
  },
  methods: {
    ...mapActions(['setSearchQuery', 'clearSearchQuery']),
    handleShowChange(showId) {
      this.clearSearchQuery();
      this.$router.push({ name: 'show.details', params: { showId } });
    },
    handleSearchQuery() {
      this.setSearchQuery(this.searchQuery);
    },
  },
};
</script>
