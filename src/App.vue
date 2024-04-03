<script setup>
import { ref, computed } from "vue";
import AppDropZone from "./components/AppDropZone.vue";

const xmlSVG = ref("");

const formattedSVG = computed(() => {
  return;
});
</script>

<template>
  <div class="app-container">
    <h1 class="text-h3 text-white text-center">SVG-encoder for Quasar app icons</h1>
    <div class="content bg-white q-pa-md">
      <div>
        <p>Insert SVG:</p>
        <q-input v-model="xmlSVG" outlined type="textarea" />
      </div>

      <div>
        <p>Ready for Quasar:</p>
        <q-input v-model="text" outlined type="textarea" readonly />
      </div>

      <div>
        <p>Preview:</p>
        <div class="content__preview">
          <q-icon :name="text" />
        </div>
      </div>
    </div>

    <AppDropZone />
  </div>
</template>

<style scoped lang="scss">
.app-container {
  display: flex;
  flex-direction: column;
  height: 100vh;
  width: 100vw;
  background-color: $dark;
}

.content {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px;
  margin: 0 auto;
  width: 100%;
  max-width: 960px;
  border-radius: 4px;

  & > div {
    display: flex;
    flex-direction: column;
    height: 200px
  }

  &__preview {
    flex: 1;
    border-radius: 4px;
    border: 1px solid rgba(0, 0, 0, 0.24);
  }
}
</style>
