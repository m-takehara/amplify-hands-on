<template>
<div>
   <input type="file" @change="identifyFromFile"/><br>
   <img width="150px" v-show="uploadedImage" :src="uploadedImage" />
    <!-- 修正箇所(1) テキスト解析結果を表示するpタグを追加する -->
    <p>{{ this.fullText }}</p>
    <div class="error">{{ error }}</div>
</div>
</template>

<script>
    // 修正箇所(2) Predictionsをインポートする
    import { Predictions } from 'aws-amplify';

    window.LOG_LEVEL = 'VERBOSE';
    export default {
        name: 'IdentifyText',
        data() {
            return {
                uploadedImage: "",
                error: "", // 修正箇所(3-1) 一つ下に行を追加するので「,」を追加
                fullText: "" // 修正箇所(3-2) テキスト解析結果を表示するステートを追加
            }
        },
        methods: {
            identifyFromFile: function(e) {
                const files = e.target.files || e.dataTransfer.files;
                if (!files) {
                    return;
                }
                this.createImage(files[0]);

                // 画像のテキストを解析する処理を追加
                Predictions.identify({
                        text: {
                            source: {
                                file: files[0]
                            },
                            type: "ALL"
                        }
                    }).then(result => this.fullText = result.text.fullText)
                    .catch(error => this.error = JSON.stringify(error))
                // ここまで

            },
            createImage(file) {
                let reader = new FileReader();
                reader.onload = (e) => {
                    this.uploadedImage = e.target.result;
                };
                reader.readAsDataURL(file);
            },
        }
    };
</script>

<style src="../assets/style.css" />
