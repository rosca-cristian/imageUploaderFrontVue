<template>
  <v-card variant="outlined" id="dropZone" style="display: flex; flex-direction: column; align-items: center;">
    <v-img style="top: 36px" src="src/assets/image.svg" height="120px" width="120px"/>
    <p style="margin-bottom: 40px; margin-top: 37px;">Drag & Drop your image here</p>
    <div v-if="uploading">
      <progress :value="uploadProgress" max="100"></progress>
      <p>{{ uploadProgress }}%</p>
    </div>
  </v-card>
</template>

<script setup>
import { ref } from 'vue';

const imageURL = ref(null);
const uploading = ref(false);
const uploadProgress = ref(0);

document.addEventListener('DOMContentLoaded', () => {
  var dropZone = document.getElementById('dropZone');

  dropZone.addEventListener('dragover', handleDragOver, false);
  dropZone.addEventListener('drop', handleFileSelect, false);
});

function handleDragOver(event) {
  event.preventDefault();
  event.dataTransfer.dropEffect = 'copy';
}

async function handleFileSelect(event) {
  event.preventDefault();

  const files = event.dataTransfer.files;
  if (files.length > 0) {
    const file = files[0];
    if (file.type.match(/^image\//)) {
      uploading.value = true;
      uploadProgress.value = 0;

      const formData = new FormData();
      formData.append('image', file);

      try {
        const response = await fetch('http://localhost:4000/api/image-uploader/upload', {
          method: 'POST',
          body: formData,
          onUploadProgress: handleUploadProgress,
        });

        if (response.ok) {
          uploading.value = false;
          uploadProgress.value = 0;
          imageURL.value = URL.createObjectURL(file);
        }
      } catch (error) {
        console.error(error);
      }
    } else {
      alert('Please drop only images.');
    }
  }
}

function handleUploadProgress(event) {
  if (event.lengthComputable) {
    const percentComplete = (event.loaded / event.total) * 100;
    uploadProgress.value = Math.round(percentComplete);
  }
}
</script>
