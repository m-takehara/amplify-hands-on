<template>
  <div>
    <input type="file" @change="identifyFromFile" /><br />
    <img width="150px" v-show="uploadedImage" :src="uploadedImage" />

    <!-- 修正箇所(1) 物体検知結果を表示させるテーブルを配置する -->
    <el-table v-if="labels.length" :data="labels">
      <el-table-column prop="name" label="Name"> </el-table-column>
      <el-table-column prop="metadata.confidence" label="Confidence[%]">
      </el-table-column>
    </el-table>
    <!-- ↑ここまで↑-->

    <div class="error">{{ error }}</div>
  </div>
</template>
<script>
    import { Predictions } from 'aws-amplify'; // 修正箇所(2) Predictionsをimportする

    window.LOG_LEVEL = 'VERBOSE';
    export default {
        name: 'ObjectDetection',
        data() {
            return {
                uploadedImage: '',
                error: '', // 修正箇所(3-1) 一つ下に行を追加するので「,」を追加
                labels: [] // 修正箇所(3-2) Object Detectionの結果を格納するステートを追加
            };
        },
        methods: {
            identifyFromFile: function(e) {
                const files = e.target.files || e.dataTransfer.files;
                if (!files) {
                    return;
                }
                this.createImage(files[0]);

                // 修正箇所(4) Object Detectionの実装を追加する
                Predictions.identify({
                        labels: {
                            source: {
                                file: files[0]
                            },
                            type: 'ALL'
                        }
                    })
                    .then(result => (this.labels = result.labels))
                    .catch(error => (this.error = JSON.stringify(error)));
                // ↑ここまで↑
            },
            createImage(file) {
                let reader = new FileReader();
                reader.onload = e => {
                    this.uploadedImage = e.target.result;
                };
                reader.readAsDataURL(file);
            }
        }
    };
</script>
