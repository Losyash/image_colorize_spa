<template>
  <label class="upload-area"
    @drop.stop.prevent="drop"
    @dropover.stop.prevent="dropover"
  >
    <span class="upload-text">Нажмите для выбора файла(ов)</span>

    <input :accept="accept"
      :multiple="multiple"
      class="upload-input"
      type="file"
      @input="change"
    />
</label>
</template>

<script setup>
  import { ref, computed } from 'vue';

  const props = defineProps({
    mimes: {
      type: Array,
      required: true
    },
    disabled: {
      type: Boolean,
      default: true
    },
    multiple: {
      type: Boolean,
      default: false
    }
  });

  const emits = defineEmits([
    'change'
  ]);

  const mimes = ref(props.mimes);

  const change = (e) => {
          emits('change', [...e.target.files]);
        }


  const accept = computed(() => {
          if (mimes && mimes.length) {
            return mimes.join(',');
          } else {
            return null;
          };
        });


        const drop = (e) => {
          const files = [];
  
          [...e.dataTransfer.files].forEach((file) => {
            if (mimes.includes(file.type)) {
              files.push(file);
            }
          });
  
          emits('change', files);
        }
  </script>
  
  <style scoped>
    .upload-area {
      align-items: center;
      border: 1px dashed #1E4391;
      display: flex;
      flex-wrap: wrap;
      height: 80px;
      justify-content: center;
      max-width: 100%;
      padding: 20px 10px;
      position: relative;
      transition: 0.2s;
      width: 100%;
      border-radius: 5px;
    }
    .upload-area:hover {
      border: 1px dashed green;
    }

    .upload-area:hover .upload-text {
        color: green
    }

    .upload-input {
      cursor: pointer;
      height: 100%;
      left: 0;
      opacity: 0;
      position: absolute;
      top: 0;
      right: 0;
      bottom: 0;
      width: 100%;
    }
    .upload-icon {
      font-size: 30px;
      color: #40a9ff;
    }
    .upload-text {
      font-family: "Roboto Regular", sans-serif;
      display: flex;
    justify-content: center;
    width: 100%;
  }
</style>