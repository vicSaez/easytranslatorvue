<template>
<h1 class="display-1">EasyTranslator v1 VUE Edition</h1>
  <va-card color="background" style="padding: 0.75rem; margin: 1.0rem">
    <div class="layout gutter--md">
    <div class="row">
      <div class="flex" style="width: 100%;">
      <va-input
      class="mb-4"
      v-model="sourceText"
      label="Source Text"
      placeholder="Enter text to translate"
      type="textarea"
      />
      </div>
    </div>
    <div class="row">
      <div class="flex" style="">
      <va-select v-model="sourceSelectValue" :options="sourceOptions" label="Source Language"/>
      </div>
      <div class="flex" style="">
      <va-select v-model="targetSelectValue" :options="targetOptions" label="Target Language"/>
      </div>
      <div class="flex" style="">
      <va-button  :disabled=!isFormValid @click="translateClick"> Translate </va-button>
      </div>
    </div>
          <div class="row">
      <div class="flex" style="width: 100%;">
      <va-input
      class="mb-4"
      v-model="targetText"
      label="Target Text"
      placeholder="Here will appear the translation"
      type="textarea"
      readonly
      />
      </div>
    </div>
    </div>

  </va-card>
</template>

<script>

export default {
  name: 'App',
  data () {
    const sourceOptions = [
      {
        text: 'Auto',
        value: 'auto',
      },
      {
        text: 'English',
        value: 'en',
      },
      {
        text: 'German',
        value: 'de',
      },
      {
        text: 'French',
        value: 'fr',
      },
    ]
    const targetOptions = [
      {
        text: 'English',
        value: 'en',
      },
      {
        text: 'German',
        value: 'de',
      },
      {
        text: 'French',
        value: 'fr',
      },
    ]



    return {
      sourceSelectValue: sourceOptions[0],
      sourceOptions,
      targetSelectValue: targetOptions[1],
      targetOptions,
      sourceText: '',
      targetText: '',
      isFormValid:false,
    }
  },

  watch: {
    sourceText(newSourceText){
      this.isFormValid = !(newSourceText=='');
    }

  },

  methods: {
        callAPI(sourceLanguage,targetLanguage,sourceText) {
      return fetch(`http://localhost:49156/api/v1/Translate?sourceLanguage=${sourceLanguage}&targetLanguage=${targetLanguage}&sourceText=${sourceText}`, { method: 'GET', mode: 'cors' })
  .then(data => data.json());

    },


    translateClick() {
      this.callAPI(this.sourceLanguage,this.targetLanguage,this.sourceText).then( (json) => {this.targetText=json.targetText;});

      
      
    },



  },

}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
