<template>
  <form v-if="!checkLogin() && !isLogin">
    <div class="row">
      <div class="mb-2" :class="block.input.col">
        <input
          type="text"
          class="form-control"
          placeholder="Username"
          aria-label="Username"
          v-model="form.username"
          @input="checkForm()"
        />
      </div>
      <div class="mb-2" :class="block.input.col">
        <input
          type="password"
          class="form-control"
          placeholder="Password"
          aria-label="Password"
          v-model="form.password"
          @input="checkForm()"
        />
      </div>
      <div class="col-12">
        <div
          class="alert mt-2"
          role="alert"
          v-if="alert.show"
          :class="alert.class"
        >
          {{ alert.message }}
        </div>
      </div>
    </div>
  </form>

  <div class="mt-0" v-if="content.show">
    <div class="card">
      <div class="card-body">
        <div class="row">
          <div class="col-12">
            <dl class="row">
              <dt class="col-12 col-md-4">Nama Lengkap</dt>
              <dd class="col-12 col-md-8">{{ content.name }}</dd>
              <dt class="col-12 col-md-4">Kelas</dt>
              <dd class="col-12 col-md-8">{{ content.grade }}</dd>
              <dt class="col-12 col-md-4">No. Presensi</dt>
              <dd class="col-12 col-md-8">{{ content.present }}</dd>
            </dl>
          </div>
        </div>

        <div class="mb-2" v-if="checkLogin() || isLogin">
          <button class="btn btn-primary" @click="logout()">Logout</button>
        </div>

        <div class="mt-4" v-if="content.embed.show">
          <iframe
            :src="content.embed.url"
            frameborder="0"
            marginwidth="0"
            marginheight="0"
            style="border: none; width: 100%; height: 75vh"
            allowfullscreen
            webkitallowfullscreen
            mozallowfullscreen
            msallowfullscreen
          >
          </iframe>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import user from './../databases/user.json';

var md5 = require('md5');
var CryptoJS = require('crypto-js');

export default {
  name: 'FormInput',
  data() {
    return {
      user: user,
      isLogin: false,
      form: {
        username: '',
        password: '',
      },
      alert: {
        show: false,
        class: 'alert-secondary',
        message: '',
      },
      content: {
        show: false,
        name: '',
        grade: '',
        present: '',
        embed: {
          show: false,
          url: '',
        },
      },
      block: {
        input: {
          col: 'col-12 col-md-12',
        },
      },
    };
  },
  methods: {
    checkForm: function () {
      let u = this.form.username;
      let p = this.form.password ? md5(this.form.password) : '';

      let user = this.getUser(u, p);

      if (user === null) {
        this.alert.show = true;
        this.content.show = false;

        if (u && p) {
          this.alert.class = 'alert-danger';
          this.alert.message =
            'Kombinasi Username dan Password salah. Mohon teliti kembali.';
        } else if (u || p) {
          this.alert.class = 'alert-info';
          this.alert.message =
            'Mohon masukkan Username & Password yang sesuai.';
        } else {
          this.alert.show = false;
        }
      } else {
        this.alert.show = false;
        this.content.show = true;

        this.content.name = user[1];
        this.content.grade = user[2];
        this.content.present = user[0];

        this.content.embed.show = false;
        if (user[3]) {
          this.content.embed.show = true;
          this.content.embed.url = user[3];
        }
      }

      if (this.content.show) {
        this.block.input.col = 'col-6 col-md-6';
      }

      return user !== null;
    },
    getUser: function (u, p) {
      let storeData = localStorage.getItem('storedData')
        ? CryptoJS.AES.decrypt(
            localStorage.getItem('storedData'),
            ',.he7l0,.'
          ).toString(CryptoJS.enc.Utf8)
        : null;
      let user = storeData ? JSON.parse(storeData) : null;
      let key = u + '|' + p;
      if (typeof this.user[key] !== 'undefined' && user === null) {
        user = this.user[key];
        localStorage.setItem(
          'storedData',
          CryptoJS.AES.encrypt(JSON.stringify(user), ',.he7l0,.').toString()
        );
      }
      this.checkLogin();

      return user;
    },
    checkLogin: function () {
      this.isLogin = localStorage.getItem('storedData') !== null;

      return this.isLogin;
    },
    logout: function () {
      localStorage.removeItem('storedData');
      this.form.password = '';
      this.checkForm();

      return true;
    },
  },
  beforeMount() {
    this.checkForm();
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped></style>
