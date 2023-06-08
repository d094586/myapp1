<script>
import axios from "axios";

export default {
  data() {
    return {
      countRowsInTable: 1000,
      countPage: 0,
      currentPage: 1,
      table: [],
      selectCountRows: [100, 500, 1000, 5000, 10000, 20000],
      favoriteItems: []
    };
  },
  mounted() {
    this.getData();
  },
  methods: {
    async getData() {
      const resp = await axios.post("http://127.0.0.1:5000/", {
        countRowsInTable: this.countRowsInTable,
        currentPage: this.currentPage,
      });
      this.table = resp["data"]["table"];
      this.countPage = resp["data"]["countPage"];
    },
    chagePage(page) {
      this.currentPage = page;
      this.getData();
    },
    changeFavorites(event, item){
      if(event.target.checked == true){
        this.favoriteItems.push(item);
      }
    }
  },
  watch: {
    countRowsInTable() {
      this.getData();
    },
  },
};
</script>

<template>
  <v-app class="vapp">
    <h3>Test app</h3>
    <div style="height: 60px; width: 200px">
      <v-select
        style="margin-bottom: 50px"
        v-model="countRowsInTable"
        label="Количество строк в таблице"
        :items="selectCountRows"
      ></v-select>
    </div>
    <div v-if="this.table.length > 0" style="display: flex; justify-content: space-between">
      <v-table class="table">
        <thead>
          <tr>
            <th class="text-left">Name</th>
            <th class="text-left">Number</th>
            <th class="text-left">Favorites</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in table" :key="item">
            <td>{{ item.name }}</td>
            <td>{{ item.number }}</td>
            <td style="width: 20px">
              <v-container fluid>
                <input type="checkbox" @change="changeFavorites($event, item)">
              </v-container>
            </td>
          </tr>
        </tbody>
      </v-table>
      <div v-if="favoriteItems.length > 0">
        <h2>Favorites</h2>
        <v-table class="table">
        <thead>
          <tr>
            <th class="text-left">Name</th>
            <th class="text-left">Number</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in favoriteItems" :key="item">
            <td>{{ item.name }}</td>
            <td>{{ item.number }}</td>
          </tr>
        </tbody>
      </v-table>
      </div>
      
    </div>
    <div v-if="countPage > 1">
      <v-btn
        v-for="page in countPage"
        :key="page"
        @click="chagePage(page)"
        style="margin: 3px"
      >
        {{ page }}
      </v-btn>
    </div>
  </v-app>
</template>

<style scoped>
</style>
