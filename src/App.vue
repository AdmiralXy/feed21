<template>
  <div id="app">
    <a href="https://github.com/AdmiralXy/feed21" class="app__badge d-flex justify-content-between align-items-center">
      <div>
        <span>
          Github
        </span>
      </div>
      <div>
        <span>
          <svg height="24" aria-hidden="true" viewBox="0 0 16 16" width="32" data-view-component="true" class="octicon octicon-mark-github v-align-middle">
            <path fill-rule="evenodd" d="M8 0C3.58 0 0 3.58 0 8c0 3.54 2.29 6.53 5.47 7.59.4.07.55-.17.55-.38 0-.19-.01-.82-.01-1.49-2.01.37-2.53-.49-2.69-.94-.09-.23-.48-.94-.82-1.13-.28-.15-.68-.52-.01-.53.63-.01 1.08.58 1.23.82.72 1.21 1.87.87 2.33.66.07-.52.28-.87.51-1.07-1.78-.2-3.64-.89-3.64-3.95 0-.87.31-1.59.82-2.15-.08-.2-.36-1.02.08-2.12 0 0 .67-.21 2.2.82.64-.18 1.32-.27 2-.27.68 0 1.36.09 2 .27 1.53-1.04 2.2-.82 2.2-.82.44 1.1.16 1.92.08 2.12.51.56.82 1.27.82 2.15 0 3.07-1.87 3.75-3.65 3.95.29.25.54.73.54 1.48 0 1.07-.01 1.93-.01 2.2 0 .21.15.46.55.38A8.013 8.013 0 0016 8c0-4.42-3.58-8-8-8z"></path>
          </svg>
        </span>
      </div>
    </a>
    <b-container class="d-flex align-items-center minh-100">
      <b-row class="py-5">
        <b-col md="12" class="text-center">

          <Header />

          <div class="feedback__header">
            Evaluated by
            <b><a class="feedback__header__link" href="javascript:void(0);">somebody</a></b>
            <b>
              <span>right now</span>
            </b>
            <b>
              <span :class="percents > 86 ? 'text-success' : 'text-danger'">
                {{ `${percents}` }}%
              </span>
            </b>
          </div>
          <div class="feedback mt-3 mb-0">
            <span>{{ text }}</span>
          </div>
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
              v-model="checkIn"
              :options="options"
          >
          </b-form-checkbox-group>
        </b-col>

        <b-col md="12">
          <b-button @click="generateFeedback" variant="primary" class="btn-generate" :disabled="!buttonActive">Сгенерировать</b-button>
        </b-col>

      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import Rating from './components/Rating.vue'
import grades from './dictionary/grades.json'
import phrases from './dictionary/phrases.json'
import combinations from './dictionary/combinations.json'

export default {
  name: 'App',
  components: {
    Header, Rating
  },
  data () {
    return {
      percents: 0,
      text: '-',
      status: 0,
      rating: 4,
      checkIn: [],
      options: [
        { text: 'Subject', value: 'subject' },
        { text: 'Makefile', value: 'makefile' },
        { text: 'Norminette', value: 'norminette' },
        { text: 'Утечки', value: 'leaks' },
        { text: 'Тесты', value: 'tests' },
        { text: 'Checklist', value: 'checklist' },
        { text: 'Знание кода', value: 'code' },
        { text: 'Структура файлов', value: 'bad_files' },
        { text: 'Запрещенная функция', value: 'forbidden_functions' },
        { text: 'Empty Git', value: 'empty_repository' },
        { text: '-42', value: 'cheating' }
      ],
      completedIgnoreList: [
        'empty_repository', 'cheating'
      ],
      failedIgnoreList: [],
      grades,
      phrases,
      combinations
    }
  },
  methods: {
    generateFeedback () {
      this.text = this.status ? this.generateGrade() : ''
      let combinations = this.status ? this.combinations.completed : this.combinations.failed
      this.shuffleArray(this.checkIn)
      let checkInText = ''
      this.checkIn.forEach(function (value) {
        if (!((this.status && this.completedIgnoreList.indexOf(value) >= 0)
            || (!this.status && this.failedIgnoreList.indexOf(value) >= 0))) {
          let checkInPart = ''
          this.getRandomElement(combinations[value]).forEach(function (combination_part) {
            checkInPart += `${this.getRandomPhrase(combination_part)} `
          }, this)
          checkInText += `${checkInPart.slice(0, -1)}, `
        }
      }, this)
      this.text += ` ${this.replaceEndByDot(this.capitalizeFirstLetter(checkInText))}`
    },
    generateGrade () {
      let grades
      let phrase = []
      if (this.rating === 5)
        grades = this.grades['excellent']
      else if (this.rating === 4)
        grades = this.grades['good']
      else if (this.rating === 3)
        grades = this.grades['satisfactory']
      else
        grades = this.grades['so-so']
      phrase.push(grades[Math.floor((Math.random() * grades.length))])
      phrase.push(this.getRandomPhrase('project'))
      this.shuffleArray(phrase)
      return `${this.capitalizeFirstLetter(phrase[0])} ${phrase[1]}.`
    },
    getRandomPhrase (key) {
      return this.getRandomElement(this.phrases[key])
    },
    getRandomElement (array) {
      let i = Math.floor((Math.random() * array.length))
      return array[i]
    },
    capitalizeFirstLetter(string) {
      return string.charAt(0).toUpperCase() + string.slice(1);
    },
    replaceEndByDot(string) {
      return string.slice(0, -1).replace(/.$/,".");
    },
    shuffleArray(array) {
      for (let i = array.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]];
      }
    }
  },
  computed: {
    buttonActive () {
      return this.checkIn.length > 0
    }
  },
  mounted() {
    let thisScope = this
    setInterval(function() {
      if (thisScope.percents + 1 > 124)
        thisScope.percents = 0
      thisScope.percents++
    }, 40);
  }
}
</script>

<style lang="scss">
  @import "./assets/scss/app.scss";

  .minh-100 {
    min-height: calc(100vh - 100px);
  }

  #app {
    position: relative;
  }

  .app__badge {
    color: #67676d;
    position: absolute;
    right: 60px;
    top: 20px;
    transition: 0.3s;
  }

  @media (max-width: 720px) {
    .app__badge {
      right: 20px;
      top: 15px;
    }
  }

  .app__badge:hover {
    text-decoration: none;
    color: #005893;
    transition: 0.3s;
  }

  .feedback {
    font-size: 16px;
    padding: 10px;
    background-color: #f7f7f7;
    border-radius: 3px;
    margin: 5px 0;
    text-align: left;
  }

  .feedback__header {
    color: #67676d;
    text-transform: uppercase;
    font-size: .8em;
    text-align: left;
  }

  .feedback__header__link {
    margin-right: 4px;
  }

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
