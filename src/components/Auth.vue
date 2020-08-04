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
                endpoint: 'http://b-sheria.dev-tooling.xyz/api/v1/',// TODO set your endpoint url here
                user: {
                    email: '',
                    password: ''
                }
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
             * --------------------------------------------------
             * Try getting the user who is authorized by the
             * token you cached.
             * -------------------------------------------------
             * */
            getUser() {
                fetch(this.endpoint + 'users', {
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
