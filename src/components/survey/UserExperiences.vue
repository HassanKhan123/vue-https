<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>
      <div>
        <base-button @click="loadExperiences"
          >Load Submitted Experiences</base-button
        >
      </div>
      <p v-if="loading">Loading...</p>
      <p v-else-if="!loading && surveyResults.length === 0">
        No stored experiences found. Start adding some survey results.
      </p>
      <ul v-else-if="!loading && surveyResults.length > 0">
        <survey-result
          v-for="result in surveyResults"
          :key="result.id"
          :name="result.name"
          :rating="result.rating"
        ></survey-result>
      </ul>
    </base-card>
  </section>
</template>

<script>
import SurveyResult from './SurveyResult.vue';

export default {
  data() {
    return {
      surveyResults: [],
      loading: false,
    };
  },
  components: {
    SurveyResult,
  },
  methods: {
    loadExperiences() {
      this.loading = true;
      fetch('https://vue-http-50019.firebaseio.com/surveys.json')
        .then((res) => {
          if (res.ok) {
            return res.json();
          }
        })
        .then((data) => {
          const results = [];
          for (const id in data) {
            results.push({ id, name: data[id].name, rating: data[id].rating });
          }
          this.surveyResults = results;
          this.loading = false;
        })
        .catch((err) => {
          console.log(err);
          this.loading = false;
        });
    },
  },
  mounted() {
    this.loadExperiences();
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