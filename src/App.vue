<template>
  <div id="app">
    <div class="app-heading mt-5">
      <h2>OCR App</h2>
      <p>Optical Character Recognition</p>
    </div>
    <div class="container">
        <div class="row">
            <div class="col-12">
                <div class="card mt-4">
                    <div class="card-header">Uplaod Image or PDF</div>
                    <div class="card-body">
                        <form action="" method="post">
                            <div class="form-group mt-2">
                                <label class="mb-3">Image</label>
                                <input type="file" class="form-control" id="image">
                            </div>
                            <div class="form-group mt-2">
                                <label class="mb-3">Language</label>
                                <select class="form-control" v-model="language">
                                    <option value="eng">English</option>
                                    <option value="ara">Arabic</option>
                                </select>
                            </div>
                            <div class="form-group mt-3 text-center">
                                <b-button variant="success" @click="convert">Convert</b-button>
                            </div>
                        </form>
                        <div class="text-center mt-4" v-if="loading">
                            <h2>Loading.....</h2>
                        </div>
                        <div class="result text-right" v-if="result">
                            <label>Result:</label>
                            <textarea class="form-control text-center mt-3" rows="10" :value="result" disabled></textarea>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
      return {
          result: "",
          loading: false,
          image: null,
          language: null
      }
  },
  methods: {
      convert() {
          this.loading = true
          const axios = require('axios');
          const fd = new FormData();
          let image = document.getElementById('image').files[0]
          fd.append("file", image)
          fd.append("language", this.language)
          fd.append("isOverlayRequired", true)
          axios({
              method: "post",
              url: "https://api.ocr.space/parse/image",
              data: fd,
              headers: {"Content-Type": "multipart/form-data", "apikey": "d61e1e3a5c88957"},
          })
          .then((res) => {
              console.log(res.data)
              this.result = res.data.ParsedResults[0].ParsedText
              this.loading = false
          })
          .catch((err) => {
              console.log(err)
          })
      }
  }
}
</script>

<style>
body {
  background-color: #f9f9f9;
}
#app {
  margin: 20px 0;
}
.app-heading {
  text-align: center;
}
</style>
