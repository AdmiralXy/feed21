<template>
  <div id="app">
    <b-container>
      <b-row>
        <b-col md="12" class="text-center">
          <img alt="Логотип" src="./assets/logo.png" class="img-fluid">
          <Header />
          <b-form-textarea
              id="textarea"
              v-model="text"
              placeholder="Enter something..."
              rows="3"
              max-rows="6"
          ></b-form-textarea>

          <pre class="mt-3 mb-0">{{ text }}</pre>
          <b-button variant="primary">Создать</b-button>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from './components/Header.vue'

export default {
  name: 'App',
  components: {
    Header
  }
}
</script>

<style lang="scss">
  @import url('https://fonts.googleapis.com/css2?family=Open+Sans:wght@300;400;700&display=swap');
  @import "./assets/scss/app.scss";
</style>
