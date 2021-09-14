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
              rows="3"
              max-rows="6"
          ></b-form-textarea>

          <pre class="mt-3 mb-0">{{ text }}</pre>
        </b-col>

        <b-col md="12" class="py-4">
          <b-form-checkbox v-model="status" :class="{ 'text-success': status, 'text-danger': !status  }" switch>
            <span v-if="status">Проект выполнен</span>
            <span v-else>Проект провален</span>
          </b-form-checkbox>
        </b-col>

        <b-col md="12" v-if="status">
          <Rating v-model="rating" />
        </b-col>

        <b-col md="12" class="pb-4">
          <p v-if="status">Что проверено?</p>
          <p v-else>В чем ошибка?</p>
          <b-form-checkbox-group
              v-model="value"
              :options="options"
          >
          </b-form-checkbox-group>
        </b-col>

        <b-col md="12">
          <b-button variant="primary" class="btn-generate" :disabled="!buttonActive">Сгенерировать</b-button>
        </b-col>

      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import Rating from './components/Rating.vue'

export default {
  name: 'App',
  components: {
    Header, Rating
  },
  data () {
    return {
      text: '',
      status: 0,
      rating: 4,
      value: [],
      options: [
        { text: 'Subject', value: 'subject' },
        { text: 'Makefile', value: 'makefile' },
        { text: 'Norminette', value: 'norminette' },
        { text: 'Утечки', value: 'leaks' },
        { text: 'Тесты', value: 'tests' },
        { text: 'Checklist', value: 'checklist' },
        { text: 'Знание кода', value: 'code' },
        { text: 'Структура файлов', value: 'bad_files' },
        { text: 'Запрещенная функция', value: 'forbidden_function' }
      ]
    }
  },
  computed: {
    buttonActive () {
      return this.value.length > 0
    }
  }
}
</script>

<style lang="scss">
  @import url('https://fonts.googleapis.com/css2?family=Open+Sans:wght@300;400;700&display=swap');
  @import "./assets/scss/app.scss";

  .btn-generate {
    width: 235px;
    height: 44px;
    margin-left: 0;
    margin-top: 0;
    margin-bottom: 18px;
    background-color: #ec3569 !important;
    border-color: #ec3569 !important;
    border-radius: 4px;
    color: #fff;
    text-decoration: none;
  }

  .btn-generate:hover {
    -webkit-box-shadow: 0 0 7px 0 #ec3569;
    -moz-box-shadow: 0 0 7px 0 #ec3569;
    box-shadow: 0 0 7px 0 #ec3569;
  }

  .btn:focus {
    box-shadow: none;
  }

  input:focus {
    box-shadow: none;
    border: none;
  }

  .custom-control-input:focus:not(:checked)~.custom-control-label::before {
    box-shadow: none;
  }

  .custom-control-input:focus~.custom-control-label::before {
    box-shadow: none;
  }
</style>
