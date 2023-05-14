<template>
  <div class="app-container">
    <el-input placeholder="Please input" v-model="locationName" class="input-with-select" style="width: 70%;"> 
    <el-button @click="changeSrc" slot="append" icon="el-icon-search"></el-button>
    </el-input>
    <iframe
      width="100%"
      height="500"
      style="border:0"
      loading="lazy"
      allowfullscreen
      referrerpolicy="no-referrer-when-downgrade"
      :src="iframeSrc">
    </iframe>
  </div>
</template>
<script>

export default {
  name: "Search",
  data() {
    return {
      locationName: "",
      iframeSrc: "https://www.google.com/maps/embed/v1/place?key=AIzaSyC8O3mVbz2sai4ktDOTDgO8Dgvpk0hFyL0&q=H%C3%A0%20n%E1%BB%99i"
    }
  },
  methods: {
    changeSrc() {
      this.iframeSrc = "https://www.google.com/maps/embed/v1/place?key=AIzaSyC8O3mVbz2sai4ktDOTDgO8Dgvpk0hFyL0&q=" + this.locationName
    }
  }
};
</script>