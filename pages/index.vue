<template>
  <v-row justify="center" align="center">
    <v-col>
      <p class="text-center text-h6 mb-5">Приход</p>
      <ol>
        <li v-for="(sum, idx) in plus_data" :key="idx">
          <span class="green--text ml-4">
            + {{ sum.toLocaleString("en-US") }}
          </span>
        </li>
      </ol>
    </v-col>
    <v-divider vertical></v-divider>
    <v-col>
      <p class="text-center text-h6 mb-5">Расход</p>
      <ol>
        <li v-for="(sum, idx) in minus_data" :key="idx">
          <span class="red--text ml-4">
            - {{ sum.toLocaleString("en-US") }}
          </span>
        </li>
      </ol>
    </v-col>
  </v-row>
</template>

<script>
export default {
  name: "IndexPage",
  data: () => ({
    minus_data: [],
    plus_data: [],
  }),
  mounted() {
    const local = localStorage.getItem("minus");
    if (local !== null) {
      const plus_data = localStorage.getItem("plus");
      this.plus_data = JSON.parse(plus_data);
      if (this.plus_data.length) {
        this.total = this.plus_data.reduce((a, b) => a + b);
        this.$store.state.plus = this.total;
      }
    }
    const minus = localStorage.getItem("minus");
    if (minus !== null) {
      const minus_data = localStorage.getItem("minus");
      this.minus_data = JSON.parse(minus_data);
      if (this.minus_data.length) {
        this.total = this.minus_data.reduce((a, b) => a + b);
        this.$store.state.minus = this.total;
      }
    }
  },
};
</script>
