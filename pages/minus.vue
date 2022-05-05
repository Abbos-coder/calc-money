<template>
  <div class="main-block container">
    <p class="text-center text-h5 mb-5">Расход</p>
    <ol v-if="!!plus_data">
      <li v-for="(sum, idx) in plus_data" :key="idx" class="text-h6">
        <span class="red--text ml-4">
          - {{ !!plus_data ? sum.toLocaleString("en-US") : null }}
        </span>
      </li>
    </ol>
    <v-divider class="my-6" />
    <div>
      <p class="text-h5 text-right">
        Total:
        <span class="red--text">
          - {{ !!plus_data ? total.toLocaleString("en-US") : total }}
        </span>
        сум
      </p>
    </div>
    <div class="add-block">
      <v-row class="mr-4">
        <v-text-field
          label="добавить расход"
          type="number"
          hide-details="true"
          v-model="new_summ"
        ></v-text-field>
        <v-btn
          color="primary"
          small
          :disabled="!new_summ"
          class="mt-auto ml-4"
          @click="addData"
        >
          добавить
        </v-btn>
      </v-row>
    </div>
    <div class="black-block"></div>
  </div>
</template>

<script>
export default {
  data: () => ({
    plus_data: [],
    total: 0,
    new_summ: null,
  }),
  watch: {
    new_summ(e) {
      return e > 0;
    },
  },
  methods: {
    addData() {
      this.plus_data.push(+this.new_summ);
      this.total = this.plus_data.reduce((a, b) => a + b);
      this.$store.state.minus = this.total;
      this.new_summ = null;
      localStorage.setItem("minus", JSON.stringify(this.plus_data));
      const plus_data = localStorage.getItem("minus");
      this.plus_data = JSON.parse(plus_data);
    },
  },
  mounted() {
    const local = localStorage.getItem("minus");
    if (local !== null) {
      const plus_data = localStorage.getItem("minus");
      this.plus_data = JSON.parse(plus_data);
      if (this.plus_data.length) {
        this.total = this.plus_data.reduce((a, b) => a + b);
        this.$store.state.minus = this.total;
      }
    }
  },
};
</script>
<style scoped>
.main-block {
  padding-bottom: 100px;
}
.black-block {
  height: 90px;
  width: 100%;
  background: #121212;
  position: fixed;
  bottom: 0;
  margin-left: -20px;
}

.add-block {
  background: #121212;
  position: fixed;
  bottom: 50px;
  width: 95%;
  z-index: 2;
}
</style>
