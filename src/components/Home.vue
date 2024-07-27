<template>
    <v-container>
      <v-row justify="center">
        <v-col cols="12" sm="6">
          <v-card class="pa-3" elevation="2">
            <h3>Upload your CSV file</h3>
            <v-file-input
              label="Choose CSV file"
              accept=".csv"
              @change="handleFileUpload"
              outlined
              dense
            ></v-file-input>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </template>
  
  <script lang="ts">
  import { defineComponent } from 'vue';
  import Papa from 'papaparse';
  
  export default defineComponent({
    name: 'Home',
    methods: {
      handleFileUpload(event: Event) {
        const target = event.target as HTMLInputElement;
        const file = target.files ? target.files[0] : null;
        if (file) {
          Papa.parse(file, {
            header: true,
            delimiter: ';',
            skipEmptyLines: true,
            complete: (results) => {
              this.$emit('data-loaded', this.groupByMonth(results.data));
            }
          });
        }
      },
      groupByMonth(data: any[]) {
        const months: { [key: string]: any[] } = {};
        data.forEach((item) => {
          const month = item.date.split('.')[1]; // Assumes date format is DD.MM.YYYY
          if (!months[month]) {
            months[month] = [];
          }
          months[month].push(item);
        });
        return months;
      }
    }
  });
  </script>
  
  <style scoped>
  #app {
    text-align: center;
    margin-top: 50px;
  }
  </style>
  