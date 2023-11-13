<template>
  <div>
    <h1>Vue pdf</h1>
    <div class="document_content">
      <label for="documentTitle">Заголовок документа:</label>
      <input v-model="title" id="documentTitle" type="text" />

      <label for="documentContent">Зміст документа:</label>
      <textarea v-model="content" id="documentContent"></textarea>

      <label for="documentImage">Фото:</label>
      <input @change="handleImageUpload" type="file" id="documentImage" accept="image/*" />

    </div>

    <!-- Контент, який буде конвертовано у PDF -->
    <div ref="pdfContent" class="content_wrap">
      <h1>{{ title }}</h1>
      <img v-if="imageUrl" :src="imageUrl" class="content_img" alt="Вибране фото" style="max-width: 300px; max-height: 300px;" />


      <p class="content">{{ content }}</p>
    </div>

    <button @click="downloadPDF" class="btn2"><span>Завантажити PDF</span></button>
  </div>
</template>

<script>
import html2pdf from 'html2pdf.js';

export default {
  data() {
    return {
      title: 'Назва документа',
      content: 'Зміст документа',
      imageUrl: null,
    };
  },
  methods: {
    handleImageUpload(event) {
      const file = event.target.files[0];
      if (file) {
        this.imageUrl = URL.createObjectURL(file);
      }
    },
    downloadPDF() {
      const pdfContent = this.$refs.pdfContent;

      const options = {
        margin: 10,
        filename: 'документ.pdf',
        image: { type: 'jpeg', quality: 0.98 },
        html2canvas: { scale: 2 },
        jsPDF: { unit: 'mm', format: 'a4', orientation: 'portrait' },
      };

      html2pdf(pdfContent, options);
    },
  },
};
</script>
<style>
h1{
  text-align: center;
  font-size: 4rem;
}
.document_content{
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}
label{
  font-size: 20px;
  padding-bottom: 10px;
}
#documentContent, input{
  border-style: none;
  outline-style: none;
  border: 2px solid;
  width: 200px;
  height: 40px;
  border-radius: 20px;
  padding-left: 10px;
}
#documentContent{
  padding-top: 10px;
}
#documentImage{
  border: none;
  border-radius: 0px;
}
.content{
  font-style: italic;
  text-align: center;
}
.content_img{
  border-radius: 30px;
  box-shadow: rgb(38, 57, 77) 0px 20px 30px -10px;
}
.content_wrap{
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}
.btn2 {
  box-sizing: border-box;
  padding: 1px;
  margin: 0 15px 15px 0;
  outline: none;
  border: 1px solid #F18230;
  border-radius: 25px;
  height: 46px;
  line-height: 0;
  font-size: 14px;
  font-weight: 500;
  text-decoration: none;
  color: #fff;
  background-color: #fff;
  position: relative;
  overflow: hidden;
  vertical-align: top;
  cursor: pointer;
  user-select: none;
  appearance: none;
  touch-action: manipulation;
  display: flex;
  margin: 0 auto;
}
.btn2 span {
  display: block;
  box-sizing: border-box;
  padding: 0 25px;   
  height: 42px;
  line-height: 38px;   
  border: 1px solid #F18230;
  border-radius: 25px;   
  font-size: 14px;
  color: #FFFFFF;
  background: linear-gradient(180deg, rgba(255, 255, 255, 0.25) 0%, rgba(255, 255, 255, 0) 100%), #F18230;
  text-align: center;
  font-weight: 600;
}
.btn2:focus-visible {
  box-shadow: 0 0 0 3px lightskyblue;
}
</style>