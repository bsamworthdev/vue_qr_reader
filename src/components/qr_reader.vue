<template>
  <div id="app">
    <div class="container">
      <div class="row">
        <div class="col-lg-9 col-sm-12">
          <div class="card bg-dark text-white">
            <div class="card-body">
              <h4 class="card-title">{{ title }}</h4>
              <form id="regForm" @submit.prevent="createQR">
                <div class="form-group">
                  <div class="container">
                    <div class="row">
                      <div class="col-lg-7 col-sm-12">
                        <input
                          type="text"
                          @keyup="validateURL"
                          v-bind:class="urlValidity + ' form-control urlInput'"
                          v-model="URL"
                          placeholder="Enter URL"
                        />
                      </div>
                      <div class="col-lg-5 col-sm-12">
                        <input
                          type="submit"
                          :disabled="urlValidity != 'valid'"
                          class="form-control btn btn-success"
                          value="Create QR Code"
                        />
                      </div>
                    </div>
                    <div class="row">
                      <div class="col-12">
                        <a v-if="created" v-bind:href="'https://' + URL">
                          <img
                            id="qrPreview"
                            v-bind:src="'https://www.qrtag.net/api/qr_4.png?url=http://' + URL"
                            class="form-control"
                            alt="qrtag"
                          />
                        </a>
                      </div>
                    </div>
                  </div>
                </div>
              </form>
            </div>
          </div>
        </div>
        <div class="col-lg-3 col-sm-12">
          <div class="card bg-dark text-white">
            <div class="card-body">
              <h4 class="card-title">History</h4>
              <div v-for="(item, index) in JSON.parse(previousURLs)" :key="index">
                <a @click="setURL(item)" href="#">{{ item }}</a>
              </div>
              <div v-if="JSON.parse(previousURLs).length == 0">
                <i>none found</i>
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
  name: "qrreader",
  props: {
    msg: String
  },
  data() {
    return {
      title: "Create QR Code",
      URL: "",
      previousURLs: "[]",
      urlValidity: "",
      created: false
    };
  },
  methods: {
    createQR: function() {
      this.created = true;
      if (this.urlValidity != "valid") {
        this.urlValidity = "invalid";
        return false;
      }

      var previousURLs = [];
      if (localStorage.previousURLs) {
        previousURLs = JSON.parse(localStorage.previousURLs);
      }

      if (previousURLs.includes(this.URL) == 0){
        previousURLs.unshift(this.URL);
      }

      this.previousURLs = JSON.stringify(previousURLs);
      localStorage.setItem("previousURLs", JSON.stringify(previousURLs));
    },
    validateURL: function(e) {
      if (e.keyCode === 13) return;
      var pattern = new RegExp(
        "^(https?:\\/\\/)?" + // protocol
        "((([a-z\\d]([a-z\\d-]*[a-z\\d])*)\\.)+[a-z]{2,}|" + // domain name
        "((\\d{1,3}\\.){3}\\d{1,3}))" + // OR ip (v4) address
        "(\\:\\d+)?(\\/[-a-z\\d%_.~+]*)*" + // port and path
        "(\\?[;&a-z\\d%_.~+=-]*)?" + // query string
          "(\\#[-a-z\\d_]*)?$",
        "i"
      ); // fragment locator
      this.urlValidity = pattern.test(this.URL) ? "valid" : "invalid";
      this.URL = this.URL.replace("https://", "").replace("http://", "");
      this.created = false;
    },
    setURL: function(url) {
      this.urlValidity = "valid";
      this.URL = url;
      this.created = true;
    }
  },
  mounted() {
    if (localStorage.previousURLs) {
      this.previousURLs = localStorage.previousURLs;
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

#regForm {
  margin: 10px auto;
  min-width: 300px;
}

.card{
  margin-bottom:10px;
}

input {
  font-size: 17px;
  font-family: arial;
  border: 1px solid #aaaaaa;
}

input.invalid {
  background-color: #ffdddd;
}

.form-group .container .row {
  padding: 5px;
}

label {
  display: block;
}

#qrPreview {
  height: 200px;
  width: 200px;
  margin-left:auto;
  margin-right:auto;
  margin-top:30px;
}

.urlInput.valid {
  background-color: rgb(228, 255, 234);
}

.urlInput.invalid {
  background-color: pink;
}
</style>
