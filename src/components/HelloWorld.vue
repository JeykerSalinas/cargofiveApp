<template>
  <div class="hello container">
    <div>
      <b-form-select v-model="selected" :options="options"></b-form-select>
    </div>

    <input
      type="text"
      class="form form-control"
      @keyup="filterData()"
      v-model="message"
    />
    <table class="table table-striped">
      <thead>
        <tr>
          <th scope="col">Id</th>
          <th scope="col">Name</th>
          <th scope="col">Country</th>
          <th scope="col">Continent</th>
          <th scope="col">Coordinates</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="d in filteredPorts" v-bind:key="d.id">
          <td>{{ d.id }}</td>
          <td>{{ d.name }}</td>
          <td>{{ d.country }}</td>
          <td>{{ d.continent }}</td>
          <td>{{ d.coordinates }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  data() {
    return {
      pageNumber: 1,
      ports: [],
      filteredPorts: [],
      message: "",
      selected: "",
      options: [
        { value: null, text: "Please select an option" },
        { value: "id", text: "Id" },
        { value: "name", text: "Name" },
        { value: "country", text: "Country" },
        { value: "continent", text: "Continent" },
        { value: "coordinates", text: "Coordinates" },
      ],
    };
  },
  methods: {
    async getData() {
      const res = await fetch(
        `http://apitest.cargofive.com/api/ports?page=${this.pageNumber}`
      );
      const data = await res.json();
      this.ports = data.data;
      this.filteredPorts = this.ports;
    },
    filterData() {
      this.filteredPorts = this.ports.filter((port) =>
        port[this.selected]
          .toString()
          .toLowerCase()
          .includes(this.message.toString().toLowerCase())
      );
      console.log(this.filteredPorts);
    },
  },
  mounted() {
    this.getData();
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss"></style>
