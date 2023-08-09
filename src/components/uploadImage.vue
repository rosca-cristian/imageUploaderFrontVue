<template>
  <v-card variant="tonal" width="469px" style="display: flex; flex-direction: column; align-items: center;">
    <h1 style="margin-top: 36px; margin-bottom:16px;">Upload your image</h1>
    <h4 style="margin-bottom: 30px;">File should be Jpeg, Png...</h4>
    <drag-and-drop style="width: 338px; height: 219px; flex-shrink: 0;"/>
    <v-card-text>Or</v-card-text>
    <v-form>
      <v-btn variant="outlined" v-on:click="showExplorer()">Choose a file</v-btn>
      <v-file-input id="fileInput" style="display: none"></v-file-input>
    </v-form>
  </v-card>
</template>
<script setup>
import DragAndDrop from "@/components/DragAndDrop.vue";

function showExplorer() {
  document.getElementById("fileInput").click();
}
</script>