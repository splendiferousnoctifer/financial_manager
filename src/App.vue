<template>
  <v-app>
    <Home @data-loaded="setData" />
    <v-container>
      <v-row>
        <v-col v-for="(value, month) in months" :key="month">
          <v-btn @click="selectMonth(month)">{{ month }}</v-btn>
        </v-col>
      </v-row>
    </v-container>
  </v-app>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import Home from './components/Home.vue';

export default defineComponent({
  components: {
    Home
  },
  data() {
    return {
      months: {} as { [key: string]: any[] }
    };
  },
  methods: {
    setData(data: { [key: string]: any[] }) {
      this.months = data;
    },
    selectMonth(month: string) {
      this.$router.push({ name: 'MonthDetail', params: { month } });
    }
  }
});
</script>
