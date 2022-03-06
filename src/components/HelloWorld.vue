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
    <div class="overflow-auto">
      <b-table
        id="my-table"
        :items="filteredPorts"
        :per-page="perPage"
        :current-page="currentPage"
        small
      ></b-table>
      <b-pagination
        v-model="currentPage"
        :total-rows="rows"
        :per-page="perPage"
        @change="onPageChange"
      ></b-pagination>
    </div>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  data() {
    return {
      perPage: null,
      currentPage: 1,
      numberOfPages: 25,
      rows: null,
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
    async getData(pageNumber) {
      const res = await fetch(
        `http://apitest.cargofive.com/api/ports?page=${pageNumber}`
      );
      const data = await res.json();
      this.ports = data.data;
      this.filteredPorts = this.ports;
      this.currentPage = data.meta.current_page;
      this.perPage = data.meta.per_page;
      this.numberOfPages = data.meta.last_page;
      this.rows = data.meta.total;
    },
    filterData() {
      this.filteredPorts = this.ports.filter((port) =>
        port[this.selected]
          .toString()
          .toLowerCase()
          .includes(this.message.toString().toLowerCase())
      );
    },
    onPageChange(page) {
      this.currentPage = page;
      this.getData(this.currentPage);
      console.log(page);
    },
  },
  mounted() {
    this.getData(this.currentPage);
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss"></style>
