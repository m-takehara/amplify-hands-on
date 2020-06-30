<template>
<div>
    <el-input class="speech-input" placeholder="読み上げたい文章を入力" v-model="inputText"></el-input>
    <el-button @click="speech" type="success" plain>スピーチ</el-button>
</div>
</template>
<div class="error">{{ error }}</div>
<script>
    // 修正箇所(1) Predictionsをimportする
    import { Predictions } from 'aws-amplify';

    window.LOG_LEVEL = 'VERBOSE';
    export default {
        name: 'TextSpeech',
        data() {
            return {
                inputText: "",
                error: ""
            }
        },
        methods: {
            speech: function() {

                // 修正箇所(2) スピーチ機能を実装する
                Predictions.convert({
                    textToSpeech: {
                        source: {
                            text: this.inputText,
                        }
                    }
                }).then(result => {
                    let AudioContext = window.AudioContext || window.webkitAudioContext;
                    const audioCtx = new AudioContext();
                    const source = audioCtx.createBufferSource();
                    audioCtx.decodeAudioData(result.audioStream, (buffer) => {
                        source.buffer = buffer;
                        source.connect(audioCtx.destination);
                        source.start(0);
                    })
                }).catch(error => this.error = JSON.stringify(error))
                // --↑ここまで↑--

            }
        }
    }
</script>

<style src="../assets/style.css" />
