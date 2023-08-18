<template>
  <div>
    <TypeRacerTextBox :word="word" @next-word="onNextWord" />
    <input type="text" v-model="word" />
  </div>
</template>

<script lang="ts" setup>
const word = ref("");
const onNextWord = () => {
  word.value = "";
};
</script>
