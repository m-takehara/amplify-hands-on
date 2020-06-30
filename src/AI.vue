<template>
  <div>
    <div class="title">
      <h2>Amplifyで作るAIアプリ</h2>
    </div>
    <div class="main-contents">
      <el-tabs v-model="activeName">
        <el-tab-pane label="step1. 翻訳" name="first"><translate /></el-tab-pane>
        <el-tab-pane label="step2. 画像の物体検知" name="second"><objectDetection /></el-tab-pane>
        <el-tab-pane label="step3. 画像のテキスト検知" name="third"><identifyText/></el-tab-pane>
        <el-tab-pane label="step4. テキスト分析" name="fourth"><textInterpretation/></el-tab-pane>
        <!-- 修正箇所(1) タグを追加する -->
        <el-tab-pane label="step5. テキスト読み上げ" name="fifth"><textSpeech/></el-tab-pane>
      </el-tabs>
    </div>
  </div>
</template>

<script>
  import Translate from './components/Translate'
  import ObjectDetection from "./components/ObjectDetection"
  import IdentifyText from "./components/IdentifyText"
  import TextInterpretation from "./components/TextInterpretation"
  // 修正箇所(2)コンポーネントをimportする
  import TextSpeech from "./components/TextSpeech"
  window.LOG_LEVEL = 'VERBOSE';
  export default {
    name: 'app',
    data() {
      return {
        activeName: 'first'
      }
    },
    components: {
      translate: Translate,
      objectDetection: ObjectDetection,
      identifyText: IdentifyText,
      textInterpretation: TextInterpretation, // 修正箇所(3-1) 一つ下に行を追加するので「,」を追加
      textSpeech: TextSpeech // 修正箇所(3-2)コンポーネントを追加する
    }
  };
</script>

<style src="./assets/style.css" />
