<template>
  <div id="image">
    <el-upload
        v-model:file-list="data.fileList"
        class="upload-demo"
        action="#"
        :on-change="handleChange"
        :before-upload="beforeFileUpload"
        :auto-upload="false"
    >
      <el-button type="primary">Click to upload</el-button>
      <template #tip>
        <div class="el-upload__tip">
          jpg/png files with a size less than 500kb
        </div>
      </template>
    </el-upload>
    <el-button @click="changePdf">开始转换</el-button>
    <img :src="data.filePath" alt="" id="img" style="display: block;">
  </div>
</template>

<script setup lang="ts">
import {reactive} from "vue";
import html2canvas from "html2canvas";
import jsPDF from 'jspdf'

const data = reactive({
  fileList: [],
  filePath: ''
})

const handleChange = (uploadFile, uploadFiles) => {
  // createLocalUrl(uploadFile)
  console.log(uploadFile)
  const reader = new FileReader();
  reader.onload = (e) => {
    const base64String = e.target.result;
    data.filePath = base64String
        console.log(base64String); // 这里是Base64编码的字符串
    // 你可以在这里使用或保存base64String
  };
  reader.readAsDataURL(uploadFile.raw);
  // console.log(data.fileList)
  // console.log(uploadFile, uploadFiles);
}

const changePdf = () => {
  html2canvas(document.getElementById('img')).then(function (canvas) {
    var pageData = canvas.toDataURL('image/jpeg', 1.0);
    var pdf = new jsPDF('', 'pt', 'a4');
    pdf.addImage(pageData, 'JPEG', 0, 0);
    pdf.save('name.pdf');
  });
}

const createLocalUrl = (uploadFile) => {
  let URL = null
  if(window.createObjectURL != undefined) {
    URL = window.createObjectURL(uploadFile)
  } else if(window.URL != undefined) {
    URL = window.URL.createObjectURL(uploadFile)
  } else if(window.webkitURL != undefined) {
    URL = window.webkitURL.createObjectURL(uploadFile)
  }
  data.filePath = URL
  console.log(URL)
}

const beforeFileUpload = (file: File) => {

}
</script>
