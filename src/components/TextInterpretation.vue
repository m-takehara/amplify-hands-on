<template>
<div>
    <el-input class="interpretation-input" placeholder="分析したい文章を入力(英語)" v-model="inputText"></el-input>
    <el-button @click="interpretation" type="success" plain>分析</el-button>
    <!-- 修正箇所(1) 解析結果を表示するテーブルを追加 -->
    <div class="result-content" v-if="textInterpretation">
      <h2>言語</h2>
      <el-table
        :data="[{lang: this.textInterpretation.language}]"
        style="width: 100%">
        <el-table-column
          prop="lang"
          label="言語"
          />
      </el-table>
      <h2>ネガポジ判定</h2>
      <el-table
        :data="[this.textInterpretation.sentiment]"
        style="width: 100%">
        <el-table-column
          prop="positive"
          label="positive"
          />
        <el-table-column
          prop="negative"
          label="negative"
          />
        <el-table-column
          prop="neutral"
          label="neutral"
          />
        <el-table-column
          prop="mixed"
          label="mixed"
          />
        <el-table-column
          prop="predominant"
          label="predominant"
          />
      </el-table>
      <h2>キーフレーズ</h2>
      <el-table
        :data="this.textInterpretation.keyPhrases"
        style="width: 100%">
        <el-table-column
          prop="text"
          label="キーフレーズ"
          />
      </el-table>
      <h2>エンティティ</h2>
      <el-table
        :data="this.textInterpretation.textEntities"
        style="width: 100%">
        <el-table-column
          prop="text"
          label="text"
          />
        <el-table-column
          prop="type"
          label="type"
          />
      </el-table>
      <h2>文法</h2>
      <el-table
        :data="this.textInterpretation.syntax"
        style="width: 100%">
        <el-table-column
          prop="text"
          label="text"
          />
        <el-table-column
          prop="syntax"
          label="syntax"
          />
      </el-table>
    </div>
    <!-- ↑ここまで↑ -->
    <div class="error">{{ error }}</div>
  </div>
</template>

<script>
    // 修正箇所(2) Predictionsをimportする
    import { Predictions } from 'aws-amplify';

    window.LOG_LEVEL = 'VERBOSE';
    export default {
        name: 'TextInterpretation',
        data() {
            return {
                inputText: "",
                textInterpretation: null,
                error: "",
            }
        },
        methods: {
            interpretation: function() {
                // 修正箇所(3) 解析機能を追加する
                Predictions.interpret({
                    text: {
                        source: {
                            text: this.inputText,
                        },
                    }
                }).then(result => {
                    this.textInterpretation = result.textInterpretation
                    console.log(this.textInterpretation)
                }).catch(error => this.error = JSON.stringify(error))
                // ↑ここまで↑
            }
        }
    };
</script>

<style src="../assets/style.css" />
