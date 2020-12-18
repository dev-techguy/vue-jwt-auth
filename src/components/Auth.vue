<template>
  <div class="row">
    <div class="col-md-2">&nbsp;</div>
    <div class="col-md-8">
      <hr>
      <h3 class="text-center">{{ msg }}</h3>
      <hr>
      <form @submit.prevent="getAccessToken()" role="form">
        <h4>--- Testing Zone ---</h4>
        <div class="form-group">
          <label for="email">Email Address</label>
          <input type="email" id="email" placeholder="Enter E-mail Address" class="form-control"
                 v-model="user.email" required>
        </div>
        <div class="form-group">
          <label for="password">Email Address</label>
          <input type="password" id="password" placeholder="XXXXXXXX" class="form-control"
                 v-model="user.password" required>
        </div>
        <div class="form-group">
          <button type="submit" class="btn btn-outline-primary btn-block mb-2"><b>LOGIN</b></button>
        </div>
      </form>
      <a href="#" class="btn btn-outline-success btn-block mb-2" @click="getUser()"><b>GET USER</b></a>
      <a href="#" class="btn btn-outline-success btn-block mb-2" @click="getRefreshToken()"><b>GET REFRESH TOKEN</b></a>
      <a href="#" class="btn btn-outline-success btn-block mb-2" @click="downloadReport()"><b>DOWNLOAD FINE</b></a>

      <hr>
      <h3 class="text-center">--- File ---</h3>
      <form @submit.prevent="uploadCatalogue()" role="form" enctype="multipart/form-data">
        <div class="form-group">
          <label for="catalogue">Upload Test</label>
          <input type="file" id="catalogue" placeholder="Upload Catalogue" class="form-control"
                 v-on:change="catalogue" required>
        </div>
      </form>

    </div>
    <div class="col-md-2">&nbsp;</div>
  </div>
</template>

<script>
export default {
  name: 'Auth',
  props: {
    msg: String
  },
  data() {
    return {
      endpoint: 'http://40.121.158.35:85/api/v1/',// TODO set your endpoint url here
      user: {
        email: '',
        password: ''
      },
      catalogue: ''
    }
  },
  methods: {
    /**
     * --------------------------
     * Fetch the access token
     * --------------------------
     * */
    getAccessToken() {
      fetch(this.endpoint + 'token', {
        method: 'post',
        body: JSON.stringify(this.user),
        headers: {
          'content-type': 'application/json',
          'Accept': 'application/json',
        }
      })
          .then(res => res.json())
          .then(data => {
            this.user.email = '';
            this.user.password = '';
            localStorage.setItem('user-token', data.data.token);// store the token in localstorage
            return localStorage.getItem('user-token');
          })
          .catch(err => {
            console.log(err);
          })
    },


    /**
     * --------------------
     * set refresh token
     * --------------------
     * */
    getRefreshToken() {
      fetch(this.endpoint + 'refresh-token', {
        method: 'get',
        headers: {
          'content-type': 'application/json',
          'Accept': 'application/json',
          'Authorization': 'Bearer ' + localStorage.getItem('user-token'),
        }
      })
          .then(res => res.json())
          .then(data => {
            localStorage.setItem('user-token', data.data.token);// store the token in localstorage
            console.log(data);
          })
          .catch(err => {
            console.log(err);
          })
    },

    /**
     * --------------------------------------------------
     * Try getting the user who is authorized by the
     * token you cached.
     * -------------------------------------------------
     * */
    getUser() {
      fetch(this.endpoint + 'user', {
        method: 'get',
        headers: {
          'content-type': 'application/json',
          'Accept': 'application/json',
          'Authorization': 'Bearer ' + localStorage.getItem('user-token'),
        }
      })
          .then(res => res.json())
          .then(data => {
            console.log(data);
          })
          .catch(err => {
            console.log(err);
          })
    },

    /**
     * ----------------------------
     * Test upload of catalogue
     * ----------------------------
     * */
    uploadCatalogue() {
      fetch(this.endpoint + 'contacts', {
        method: 'post',
        headers: {
          'content-type': 'application/json',
          'Accept': 'application/json',
          'Authorization': 'Bearer ' + localStorage.getItem('user-token'),
        }
      })
          .then(res => res.json())
          .then(data => {
            console.log(data);
          })
          .catch(err => {
            console.log(err);
          })
    },

    /**
     * ----------------------------
     * Test download of file
     * @todo that is the report
     * ----------------------------
     * */
    downloadReport() {
      fetch(this.endpoint + 'dashboard/report', {
        method: 'get',
        headers: {
          'content-type': 'application/json',
          'Accept': 'application/json',
          'Authorization': 'Bearer ' + localStorage.getItem('user-token'),
        }
      })
          .then(res => res.json())
          .then(data => {
            console.log(data);
          })
          .catch(err => {
            console.log(err);
          })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
