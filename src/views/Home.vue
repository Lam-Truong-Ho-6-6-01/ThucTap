
<template>
  <v-app id="inspire">
    <v-content>
      <v-container class="fill-height" fluid>
        <v-row justify="center">
          <v-col cols="12" sm="8" md="8">
            <v-card class="elevation-12">
              <v-window v-model="step">
                <v-window-item :value="1">
                  <v-row>
                    <v-col cols="12" md="8">
                      <v-card-text class="mt-12">
                        <h1 class="text-center display-2 teal--text text--accent-3">Sign in</h1>
                        <div class="text-center mt-4">
                          <v-btn class="mx-2" fab color="black" outlined>
                            <v-icon>fab fa-facebook-f</v-icon>
                          </v-btn>

                          <v-btn class="mx-2" fab color="black" outlined>
                            <v-icon>fab fa-google-plus-g</v-icon>
                          </v-btn>
                          <v-btn class="mx-2" fab color="black" outlined>
                            <v-icon>fab fa-linkedin-in</v-icon>
                          </v-btn>
                        </div>
                        <h4 class="text-center mt-4">Hãy chắc chắn rằng email của bạn đã đăng ký</h4>
                        <v-form>
                          <v-text-field label="Account" name="Account" prepend-icon="email" type="text"
                            color="teal accent-3" v-model="taiKhoan" />

                          <v-text-field :type="passwordShow ? 'text' : 'password'" id="password" label="Password"
                            name="password" prepend-icon="lock" color="teal accent-3" v-model="matKhau"
                            :append-icon="passwordShow ? 'mdi-eye' : 'mdi-eye-off'"
                            @click:append="passwordShow = !passwordShow" />
                        </v-form>
                        <router-link style="text-decoration: none;" to="/fogotpassword">
                          <h3 class="text-center mt-4">Bạn đã quên mật khẩu ?</h3>
                        </router-link>
                      </v-card-text>
                      <div class="text-center mt-3">
                        <v-btn rounded color="teal accent-3" v-on:click="login" dark>SIGN IN</v-btn>
                      </div>
                    </v-col>
                    <v-col cols="12" md="4" class="teal accent-3">
                      <v-card-text class="white--text mt-12">
                        <h1 class="text-center display-1">Hello, Friend!</h1>
                        <h5 class="text-center">Đăng nhập để được sử dụng các dịch vụ của chúng tôi </h5>
                      </v-card-text>
                      <div class="text-center">
                        <v-btn rounded outlined dark>SIGN UP</v-btn>
                      </div>
                    </v-col>
                  </v-row>
                </v-window-item>
              </v-window>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
import axios from 'axios'
import { encodeBase64 } from '@progress/kendo-file-saver'
export default {
  name: "Login-form",
  data() {

    return {
      passwordShow: false,
      taiKhoan: '',
      matKhau: ''
    }
  },

  methods: {
    async login() {
      await axios.post("https://api-cokyvina.vnpttravinh.vn/xac-thuc/dang-nhap", {
        taiKhoan: this.taiKhoan,
        matKhau: encodeBase64(this.matKhau)
      })
        .then(response => {
          localStorage.setItem('accessToken', JSON.stringify(response));
          console.log(response.data.data.accessToken);
          this.$router.push({ path: '/about' })
          // localStorage.setItem('accessToken', response.data.accessToken
          // );
          // commit('loginStop', null);
          // commit('updateAccessToken', response.data.token);
          // console.log(response.data);
        })


    }

  }
};


</script>