<template>
  <section>
    <h2>Basic calculations statistics:</h2>
    <div>
      <div v-if="isLoading">
        <p>Statistic is loading...</p>
      </div>
      <div v-else>
        <p><b>Total calculations: </b><br />{{ stats.totalCalculations }}</p>
        <p><b>Total base amount: </b><br />{{ stats.totalBaseAmount }} $</p>
        <p><b>Average base amount: </b><br />{{ stats.averageBaseAmount.toFixed(2) }} $</p>
        <p>
          <b>Total amount to be repayed: </b><br />{{ stats.totalAmount.toFixed(2) }} $
        </p>
        <p>
          <b>Average amount to be repayed: </b><br />{{ stats.averageAmount.toFixed(2) }} $
        </p>
        <p><b>Total installments: </b><br />{{ stats.totalInstallment.toFixed(2) }} $</p>
        <p>
          <b>Average installments: </b><br />{{ stats.averageInstallment.toFixed(2) }} $
        </p>
        <p><b>Total length of loan: </b><br />{{ stats.totalLength }} years</p>
        <p>
          <b>Average length of loan: </b><br />{{ stats.averageLength.toFixed(2) }} years
        </p>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  data() {
    return {
      stats: {},
      loading: true,
    };
  },
  computed: {
    isLoading() {
      return this.loading;
    },
  },
  methods: {
    loadStats() {
      return (this.stats = this.$store.getters["calculations/getStats"]);
    },
    async loadCalculations() {
      try {
        await this.$store.dispatch("calculations/loadCalculations");
      } catch (error) {
        this.error =
          error.message + " in getting calculations." ||
          "Something went wrong!";
      }
    },
  },
  created() {
    this.loadCalculations();
    setTimeout(() => {
      this.stats = this.loadStats();
      this.loading = false;
    }, 500);
  },
};
</script>
