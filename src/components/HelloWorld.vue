<template>
  <div class="container">
    <div class="d-flex align-items-center justify-content-center">
      <img
        src="https://cargofive.com/wp-content/uploads/2018/07/logo.svg"
        alt="Logo cargofive"
        class="w-25"
      />
    </div>
    <div class="d-flex align-items-center justify-content-end my-4">
      <div>
        <b-form-select
          class="py-2 me-3"
          v-model="selected"
          :options="options"
        ></b-form-select>
      </div>
      <div class="w-25">
        <input
          type="text"
          class="form form-control"
          @keyup="filterData()"
          v-model="message"
          :disabled="selected === null"
          placeholder="Search"
        />
      </div>
    </div>

    <div class="overflow-auto">
      <table class="table table-ligth table-striped">
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
      <b-pagination
        v-model="currentPage"
        :total-rows="rows"
        :per-page="perPage"
        @change="onPageChange"
        align="center"
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
      selected: null,
      options: [
        { value: null, text: "Filter by" },
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
      this.ports.map((a) => {
        a[this.selected] === null ? (a[this.selected] = "") : a[this.selected];
      });
      this.filteredPorts = this.ports.filter((port) =>
        port[this.selected]
          .toString()
          .toLowerCase()
          .includes(this.message.toString().toLowerCase().trim())
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
