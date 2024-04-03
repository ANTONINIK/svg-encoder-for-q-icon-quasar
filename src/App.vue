<script setup>
import { ref, computed, watchEffect } from "vue";
import { useQuasar } from "quasar";
import { appGitHub } from "./icons";
import example from "./example";
import AppDropzone from "./components/AppDropZone.vue";
import extractSvg from "./parser";

const $q = useQuasar();
const xmlSVG = ref("");
const inlineSVG = ref({});
const iconName = ref("NO_NAME");

watchEffect(() => {
  try {
    if (xmlSVG.value && iconName.value) {
      inlineSVG.value = extractSvg(xmlSVG.value, iconName.value);
    }
  } catch (error) {
    console.log(error);
    inlineSVG.value = {};
    $q.notify({
      message: "Wrong SVG format (check console for more details)",
      color: "negative",
      timeout: 500,
    });
  }
});

const definitionSVG = computed(() => {
  if (inlineSVG.value.svgDef && inlineSVG.value.typeDef) {
    return `${inlineSVG.value.svgDef}\n\n${inlineSVG.value.typeDef}`;
  }
});

const definitionSVGInput = ref();
function copyToClipboard() {
  if (definitionSVGInput.value) {
    const el = definitionSVGInput.value.$el.querySelector("textarea");
    el.select();
    el.setSelectionRange(0, 99999);
    navigator.clipboard.writeText(el.value);
    $q.notify({
      message: "Copied to clipboard",
      color: "primary",
      timeout: 500,
    });
  }
}
</script>

<template>
  <q-scroll-area dark  style="height: 100vh">
    <div class="app-container">
      <q-btn
        :icon="appGitHub"
        size="xs"
        round
        flat
        color="white"
        class="github-btn"
        href="https://github.com/ANTONINIK/svg-encoder-for-q-icon-quasar"
      ></q-btn>

      <h1 class="text-h4 text-white text-center q-ma-none q-pb-md">
        SVG-encoder for Q-Icon (Quasar)
      </h1>
      <div class="content bg-white q-pa-md">
        <div class="column">
          <div class="content__header">
            <p class="content__label">Insert SVG:</p>
            <q-btn dense flat color="primary" @click="xmlSVG = example">Example</q-btn>
          </div>
          <q-input
            class="app-textarea"
            v-model="xmlSVG"
            hint='Example: <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512">...</svg>'
            outlined
            type="textarea"
            spellcheck="false"
          />
        </div>

        <div>
          <div class="content__header">
            <p class="content__label">Enter icon name:</p>
          </div>
          <q-input v-model="iconName" dense outlined type="text" spellcheck="false" />
        </div>

        <div>
          <div class="content__header">
            <p class="content__label">Ready for Quasar:</p>
            <q-btn @click="copyToClipboard" dense flat color="primary" :disable="!definitionSVG"
              >Copy
            </q-btn>
          </div>
          <q-input
            ref="definitionSVGInput"
            :model-value="definitionSVG"
            outlined
            type="textarea"
            readonly
            spellcheck="false"
          />
        </div>

        <div class="content__preview">
          <p class="content__label">Preview:</p>
          <div class="content__area">
            <q-icon :name="inlineSVG.svgCode" />
          </div>
        </div>
      </div>
      <AppDropzone @upload="xmlSVG = $event" />
    </div>
  </q-scroll-area>
</template>

<style lang="scss">
html {
  overflow-x: hidden;
}

body {
  background-color: $dark;
}

.q-icon {
  width: unset;
  height: unset;
}

.q-textarea .q-field__native {
  padding-top: 5px;
}

.app-textarea .q-field__inner .q-field__control {
  height: 100% !important;
}
</style>

<style scoped lang="scss">
.app-container {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  min-width: 100vw;
  padding: 12px;
  // overflow-y: scroll;
}

.app-textarea {
  flex: 1;
}

.github-btn {
  position: absolute;
  top: -115px;
  right: -100px;
  transform: scale(0.18);

  @media screen and (max-width: 700px) {
    display: none;
  }
}

.content {
  display: flex;
  flex-direction: column;
  margin: 0 auto;
  width: 100%;
  max-width: 1200px;
  border-radius: 4px;
  flex: 1;
  gap: 16px;

  & > div:first-child {
    flex: 1;
  }

  &__header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    height: 30px;
    margin-bottom: 4px;
  }

  &__label {
    margin-bottom: 4px;
    font-weight: 600;
  }

  & > div {
    display: flex;
    flex-direction: column;
  }

  &__area {
    min-height: 100px;
    border-radius: 4px;
    border: 1px solid rgba(0, 0, 0, 0.24);
    border-style: dashed;
    transition: border-color 0.36s cubic-bezier(0.4, 0, 0.2, 1);

    &:hover {
      border-color: #000;
    }
  }
}
</style>
