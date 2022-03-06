<template>
  <div class="hello container">
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
        <tr v-for="d in ports" v-bind:key="d.id">
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
      message: "",
    };
  },
  methods: {
    async getData() {
      const res = await fetch(
        `http://apitest.cargofive.com/api/ports?page=${this.pageNumber}`
      );
      const data = await res.json();
      this.ports = data.data;
      console.log(this.ports);
    },
    filterData() {
      console.log(this.ports.filter((port) => port.name === this.message));
    },
  },
  mounted() {
    this.getData();
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss"></style>
