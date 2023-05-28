
<script >
import axios from 'axios';
import { encodeBase64 } from '@progress/kendo-file-saver';
const token = JSON.parse(localStorage.getItem('accessToken'));
// console.log(token.data.data.accessToken);
const config = {
    headers: {
        'x-access-token': token.data.data.accessToken,
    },
};
export default ({
    name: "ForgotPassword",
    data() {
        return {
            matKhauHienTai: '',
            matKhauMoi: '',
            xacNhanMatKhauMoi: '',
            password1: [
                v => !!v || 'Nhập mật khẩu',
                v => (v.length <= 20 && v.length > 6) || 'Mật khẩu phải từ 6 đến 20 ký tự',
            ],
            password2: [
                v => !!v || 'Nhập mật khẩu',
                v => (v.length <= 20 && v.length > 6) || 'Mật khẩu phải từ 6 đến 20 ký tự',
                v => /(?=.*\d)(?=.*[a-z]).{6,}/.test(v) || 'Mật khẩu phải chứa ít nhất một chữ thường, một số !',
            ],
            password3: [
                v => !!v || 'Nhập mật khẩu',
                v => (v.length <= 20 && v.length > 6) || 'Mật khẩu phải từ 6 đến 20 ký tự'
            ]
        };

    },
    methods: {

        async ChangePassword() {
            await axios.put('https://api-cokyvina.vnpttravinh.vn/nguoi-dung/doi-mat-khau', {
                matKhauHienTai: encodeBase64(this.matKhauHienTai),
                matKhauMoi: encodeBase64(this.matKhauMoi),
                xacNhanMatKhauMoi: encodeBase64(this.xacNhanMatKhauMoi),
            },
                config

            )
                .then((response) => {
                    console.log(response);
                    alert('Doi mat khau thanh cong')
                })
                .catch((error) => {
                    console.log(error);
                });

        },

        ResetPassword() {
            this.matKhauHienTai = '',
                this.matKhauMoi = '',
                this.xacNhanMatKhauMoi = ''
            // this.password1 = '',
            // this.password2 = '',
            // this.password3 = ''
        }
    },
    computed: {
        passwordConfirmationRule() {
            return () =>
                this.matKhauMoi === this.xacNhanMatKhauMoi || "Xác nhận mật khẩu không chính xác ";
        }
    }
})
</script>
<template>
    <v-app id="inspire">
        <v-content>
            <v-container class="fill-height" fluid>
                <v-row justify="center">
                    <v-col cols="12" sm="8" md="8">
                        <v-card class="elevation-12">
                            <v-window v-model="step">
                                <v-window-item :value="2">
                                    <v-row class="fill-height">
                                        <v-col cols="12" md="4" class="teal accent-3">
                                            <v-card-text class="white--text mt-12">
                                                <h1 class="text-center display-1">Welcome Back!</h1>
                                                <h5 class="text-center">To Keep connected with us please login with your
                                                    personnel info</h5>
                                            </v-card-text>
                                            <div class="text-center">
                                                <router-link style="text-decoration: none" to="/"><v-btn rounded outlined
                                                        dark @click="step--">Sign
                                                        in</v-btn></router-link>
                                            </div>
                                        </v-col>

                                        <v-col cols="12" md="8">
                                            <v-card-text class="mt-12">
                                                <h1 class="text-center display-2 teal--text text--accent-3">Change Password
                                                </h1>
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
                                                <h4 class="text-center mt-4">Ensure your email for registration</h4>
                                                <v-form>
                                                    <v-text-field v-model="matKhauHienTai" label="Password" name="password1"
                                                        prepend-icon="lock" type="password" color="teal accent-3"
                                                        :rules="password1" />
                                                    <v-text-field v-model="matKhauMoi" label="New Password" name="password2"
                                                        prepend-icon="lock" type="password" color="teal accent-3"
                                                        :rules="password2" />

                                                    <v-text-field v-model="xacNhanMatKhauMoi" id="password"
                                                        label="Compare Password" name="password3" prepend-icon="lock"
                                                        type="password" color="teal accent-3"
                                                        :rules="password3.concat(passwordConfirmationRule)" />
                                                </v-form>
                                            </v-card-text>
                                            <div class="text-center mt-n5">
                                                <v-btn rounded color="teal accent-3" v-on:click="ChangePassword" dark>CHANGE
                                                </v-btn>
                                                <v-btn rounded color="red" v-on:click="ResetPassword" dark>Reset</v-btn>
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
