<template>
  <div class="container">
    <div class="row py-5">
      <div class="col">
        <h2 class="page-caption">Раскрашивание изображений с использованием нейронных сетей</h2>
      </div>
    </div>

    <div class="card p-4 mb-4">
      <div class="row">
        <div class="col-12 d-flex justify-content-start pb-3">
          <h5 class="section-caption">Выберите способ загрузки изображения</h5>
        </div>

        <div class="col-auto">
          <u-radio v-model="dataType"
            title="Загрузить из файла"
            value="file"
          />
        </div>

        <div class="col-auto">
          <u-radio v-model="dataType"
            title="Загрузить по ссылке"
            value="link"
          />
        </div>
      </div>
    </div>
    
    <div class="card p-4">
      <div class="row pb-4">
        <div class="col-12 d-flex justify-content-start pb-3">
          <h5 class="section-caption">{{ dataType === 'link' ? 'Выберите ссылку' : 'Выберите файл' }}</h5>
        </div>

        <div v-if="dataType === 'link'" class="col-12 d-flex justify-content-start pb-3">
          <input v-model="link"
            class="u-form-input"
            placeholder="Вставьте ссылку на изображение"
            type="text"
          />
        </div>

        <div v-if="dataType === 'file'"
          class="col-12 d-flex justify-content-start"
        >
          <u-file :multiple="multiple"
            :mimes="mimes"
            @change="selectFile"
          />
        </div>
      </div>

      <div class="row d--flex justify-content-end pb-4">
        <div class="col-auto">
          <button style="color: #ffffff; background-color: #C0231F;"
            @click="upload"
          >Очистить</button>
        </div>

        <div class="col-auto">
          <button style="color: #ffffff; background-color: #1E4391;"
            @click="upload"
          >Загрузить</button>
        </div>
      </div>

      <img :src="outputImage" alt="">
    </div>
  </div>
</template>

<script setup>
  import { ref } from 'vue';
  import axios from 'axios';
  import utils from '@/libs/utils';
  import URadio from '@/components/controls/URadio';
  import UFile from '@/components/controls/UFile';

  const mimes = ['image/png', 'image/jpeg'];
  const multiple = false;

  const dataType = ref('file');
  const files = ref(null);
  const link = ref(null);

  const outputImage = ref(null);

  const selectFile = (data) => files.value = data;

  const upload = async () => {
    try {
      if (dataType.value === 'file') {
        const uintdata = await utils.readFileAsArrayBuffer(files.value[0]);
        const filedata = btoa(utils.arrayToString(uintdata))

        const { data } = await axios.post('/file', { file: filedata });
        
        outputImage.value = `data:image/jpeg;base64,${data}`
      };

      if (dataType.value === 'link') {
        const { data } = await axios.post('/link', { link: link.value });
      };
    } catch (error) {
      console.log(error)
    };
  };
</script>

<style>
  @import 'bootstrap/dist/css/bootstrap.css';
  @import "@/assets/style/style.css";
</style>