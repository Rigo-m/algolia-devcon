<template>
  <AisInstantSearch :configuration :widgets>
    <div class="left-panel">
      <ais-clear-refinements />
      <h2>Brands</h2>
      <ais-refinement-list attribute="brand" searchable />
      <ais-configure :hitsPerPage="8" />
    </div>
    <div class="right-panel">
      <ais-search-box />
      <ais-hits>
        <template v-slot:item="{ item }">
          <h2>{{ item.name }}</h2>
          <img :src="item.image" align="left" :alt="item.name" />
          <div class="hit-name">
            <ais-highlight attribute="name" :hit="item"></ais-highlight>
          </div>
          <div class="hit-description">
            <ais-highlight attribute="description" :hit="item"></ais-highlight>
          </div>
          <div class="hit-price">{{ item.price }}</div>
        </template>
      </ais-hits>
      <ais-pagination />
    </div>
  </AisInstantSearch>
</template>
<script setup lang="ts">
import algoliasearch from "algoliasearch";
import "instantsearch.css/themes/algolia-min.css";

const client = algoliasearch("latency", "6be0576ff61c053d5f9a3225e2a90f76");

const algoliaRouter = useAisRouter();

const configuration = ref({
  indexName: "instant_search",
  routing: algoliaRouter.value,
  searchClient: client,
});
const widgets = computed(() => [
  useAisSearchBox({}),
  useAisClearRefinements({}),
  useAisRefinementList({ attribute: "brand" }),
  useAisConfigure({ searchParameters: { hitsPerPage: 8 } }),
  useAisHits({}),
  useAisPagination({}),
]);
</script>

<style>
body {
  font-family: sans-serif;
  padding: 1em;
}

.ais-Hits-list {
  margin-top: 0;
  margin-bottom: 1em;
}

.ais-InstantSearch {
  display: grid;
  grid-template-columns: 1fr 4fr;
  grid-gap: 1em;
}
.ais-Hits-item img {
  max-width: calc(100% - 2rem);
  margin: 1rem;
}
.hit-name {
  margin-bottom: 0.5em;
}
.hit-description {
  color: #888;
  font-size: 0.8em;
  margin-bottom: 0.5em;
}
</style>
