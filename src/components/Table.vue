<template>
  <div class="search">
    <form v-on:submit.prevent="getResult(query)">
      <input class="color" type="text" placeholder="Search" v-model="query" />
    </form>

    <div class="results" v-if="results">
      <table id="customers">
        <tr>
          <th>Name</th>
          <th>Date</th>
          <th>Price</th>
          <th>Img</th>
          <th>Detail</th>
        </tr>
        <tr v-for="result in sortedArray" :key="result.name">
          <td>{{ result.name }}</td>
          <td>{{ result.dates.start.dateTime }}</td>
          <td>-</td>
          <td>
            <img class="table-image" v-bind:src="result.images[0].url" alt="" />
          </td>
          <td><a :href="result.url" target="_blank">More Detail</a></td>
        </tr>
      </table>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "Table",
  el: "#app",
  data() {
    return {
      query: "",
      results: "",
    };
  },
  methods: {
    getResult(query) {
      axios
        .get(
          "https://app.ticketmaster.com/discovery/v2/events?apikey=7elxdku9GGG5k8j0Xm8KWdANDgecHMV0&keyword=" +
            query +
            "&locale=*"
        )
        .then((response) => {
          console.log(response.data._embedded.events[0].images[0].url);
          console.log(response.data._embedded.events);
          this.results = response.data._embedded.events;
        });
    },
  },
  computed: {
    sortedArray: function() {
      function compare(a, b) {
        if (a.name < b.name)
          return -1;
        if (a.name > b.name)
          return 1;
        return 0;
      }
      return this.results.sort(compare);
    }
  },
};
</script>


<style scoped>
h1,
h2 {
  font-weight: bold;
  color: orange;
}
.results img {
  height: 300px;
  margin-bottom: 10px;
  margin-top: 50px;
}

.color {
  color: black;
  font-weight: bold;
  background-color: orange;
}

#customers {
  margin-top: 30px;
  font-family: Arial, Helvetica, sans-serif;
  border-collapse: collapse;
  width: 100%;
}

.table-image {
  width: 100%;
  height: 100%;
  max-height: 100px;
  max-width: 200px;
}

#customers td,
#customers th {
  border: 1px solid #ddd;
  padding: 8px;
}

#customers tr:nth-child(even) {
  background-color: #f2f2f2;
}
#customers tr:hover {
  background-color: #ddd;
}
#customers th {
  padding-top: 12px;
  padding-bottom: 12px;
  text-align: left;
  background-color: #4caf50;
  color: white;
}
</style>

