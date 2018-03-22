<template>
  <div id="app">
    <ob-applicant-table :applicantData="applicantData"></ob-applicant-table>
  </div>
</template>

<script>
import ApplicantTable from './components/ApplicantTable.vue'
import data from './data.json';

export default {
  name: 'App',
  components: {
    'ob-applicant-table': ApplicantTable
  },
  data() {
    return {
      applicantData: {}
    }
  },
  mounted() {
    this.applicantData = data;
  }
}
</script>

<style>

</style>
