<template>
  <v-app dark>
    <v-navigation-drawer v-model="drawer" :clipped="clipped" fixed app>
      <v-list>
        <v-list-item
          v-for="(item, i) in items"
          :key="i"
          :to="item.to"
          router
          exact
        >
          <v-list-item-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title v-text="item.title" />
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar :clipped-left="clipped" fixed app>
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <v-spacer />
      <v-toolbar-title>
        Total:
        <span class="main-total green--text" v-if="Math.sign(calcFull) == 1">
          {{ !!calcFull ? calcFull.toLocaleString("en-US") : calcFull }}
        </span>
        <span class="main-total red--text" v-else>
          {{ !!calcFull ? calcFull.toLocaleString("en-US") : calcFull }}
        </span>
        сум
      </v-toolbar-title>
      <v-spacer />
      <v-btn color="warning" small @click="openDialog">reset</v-btn>
    </v-app-bar>
    <v-dialog v-model="dialog" persistent max-width="290">
      <v-card>
        <v-card-title class="text-h5"> Предупреждения </v-card-title>
        <v-card-text> Вы действительно хотите очистить историю ? </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="green darken-1" text @click="dialog = false">
            отмена
          </v-btn>
          <v-btn color="red darken-1" text @click="reset"> очистить </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <v-main>
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: "DefaultLayout",
  data() {
    return {
      dialog: false,
      clipped: false,
      drawer: false,
      fixed: false,
      items: [
        {
          icon: "mdi-home",
          title: "Главное",
          to: "/",
        },
        {
          icon: "mdi-plus",
          title: "Приход",
          to: "/plus",
        },
        {
          icon: "mdi-minus",
          title: "Расход",
          to: "/minus",
        },
      ],
      title: 0,
    };
  },
  computed: {
    calcFull() {
      return this.$store.state.plus - this.$store.state.minus;
    },
  },
  methods: {
    openDialog() {
      this.dialog = true;
    },
    reset() {
      localStorage.clear();
      location.reload();
      this.dialog = false;
    },
  },
  mounted() {
    const mainTotal = document.querySelector(".main-total").textContent;
    console.log(mainTotal);
    console.log(Math.sign(+mainTotal));

    // const plus = localStorage.getItem("plus");
    // const minus = localStorage.getItem("minus");
    // const plus_sum = JSON.parse(plus);
    // const minus_sum = JSON.parse(minus);
    // const plus_full = plus_sum.reduce((a, b) => a + b);
    // const minus_full = minus_sum.reduce((a, b) => a + b);
    // const total = plus_full - minus_full;
    // this.title = total;
  },
};
</script>
