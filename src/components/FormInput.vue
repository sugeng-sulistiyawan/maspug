<template>
  <form>
    <div class="row">
      <div class="col">
        <input
          type="text"
          class="form-control"
          placeholder="Username"
          aria-label="Username"
          v-model="form.username"
        />
      </div>
      <div class="col">
        <input
          type="password"
          class="form-control"
          placeholder="Password"
          aria-label="Password"
          v-model="form.password"
        />
      </div>
      <div class="col-12">
        <div
          class="alert mt-3"
          role="alert"
          v-if="form.alert.show"
          :class="form.alert.class"
        >
          {{ form.alert.message }}
        </div>
      </div>
    </div>
  </form>

  <div class="mt-0" v-if="content.show">
    <hr />
    <div class="card">
      <div class="card-body">
        <div class="row">
          <div class="col-12">
            <dl class="row">
              <dt class="col-4">Nama Lengkap</dt>
              <dd class="col-8">{{ content.name }}</dd>
              <dt class="col-4">Kelas</dt>
              <dd class="col-8">{{ content.class }}</dd>
              <dt class="col-4">No. Presensi</dt>
              <dd class="col-8">{{ content.present }}</dd>
            </dl>
          </div>
        </div>

        <div class="mt-2" v-if="content.embed.show">
          <iframe
            src="{{ content.embed.url }}"
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
export default {
  name: 'FormInput',
  data() {
    return {
      form: {
        username: '',
        password: '',
        alert: {
          show: true,
          class: 'alert-info',
          message: 'aaaaaaaa',
        },
      },
      content: {
        show: true,
        name: '',
        class: '',
        present: '',
        embed: {
          show: true,
          url: 'https://forms.office.com/Pages/ResponsePage.aspx?id=DQSIkWdsW0yxEjajBLZtrQAAAAAAAAAAAAN__r4G1WFUMlRWTksyR1hTUFpKNkpMRlo4RU8zQkxPNS4u&embed=true',
        },
      },
    };
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped></style>
