<template>
  <div>
    <el-input class="translate-input" placeholder="Input text you want to translate (in English)" v-model="inputText"></el-input>
    <el-button @click="translate" type="success" plain>翻訳</el-button>
    <p v-if="translatedText">
        {{ translatedText }}
    </p>
    <div class="error">{{ error }}</div>
  </div>
</template>
<script>
    import { Predictions } from 'aws-amplify'; // 修正箇所(1) Predictionsをimportする

    window.LOG_LEVEL = 'VERBOSE';

    export default {
        name: 'Translate',
        data() {
            return {
                inputText: "",
                translatedText: "",
                error: ""
            }
        },
        methods: {
            translate: function() {
                // 修正箇所(2) 翻訳機能を追加する
                Predictions.convert({
                    translateText: {
                        source: {
                            text: this.inputText,
                        },
                    }
                }).then(result => {
                    this.translatedText = result.text
                }).catch(error => this.error = JSON.stringify(error))
            }
        }
    }
</script>

<style src="../assets/style.css" />
