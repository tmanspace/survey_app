<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>
      <div v-if="!isLoading">
        <base-button @click="fetchResults"
          >Load Submitted Experiences</base-button
        >
      </div>
      <div v-if="isLoading">Loading...</div>
      <p v-else-if="error && !isLoading">{{ error }}</p>
      <p v-else-if="!isLoading && (!results || results.length === 0)">
        No surveys found... <br />Enter one!
      </p>
      <ul v-else-if="!isLoading && results && results.length > 0">
        <survey-result
          v-for="result in results"
          :key="result.id"
          :name="result.name"
          :rating="result.rating"
        ></survey-result>
      </ul>
    </base-card>
  </section>
</template>

<script>
import SurveyResult from "./SurveyResult.vue";

export default {
  components: {
    SurveyResult,
  },
  data() {
    return {
      results: [],
      isLoading: false,
      error: null,
    };
  },
  methods: {
    fetchResults() {
      this.isLoading = true;
      fetch(
        "https://vue-course-survey-app-91797-default-rtdb.europe-west1.firebasedatabase.app/surveys.json"
      )
        .then((response) => {
          if (response.ok) {
            return response.json();
          }
        })
        .then((data) => {
          // console.log(data);
          const results = [];
          for (const id in data) {
            results.push({
              id: id,
              name: data[id].name,
              rating: data[id].rating,
            });
          }
          this.results = results;
          this.isLoading = false;
          this.error = null;
        })
        .catch((err) => {
          console.log(err);
          this.error = "Failed to load data. Try again later.";
          this.isLoading = false;
        });
    },
  },
  mounted() {
    this.fetchResults();
  },
};
</script>

<style scoped>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
</style>
