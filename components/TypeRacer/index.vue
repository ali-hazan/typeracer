<template>
  <div>
    <TypeRacerTextBox :word="word" @next-word="onNextWord" @is-wrong="onWrong" @is-success="onSucess" />
    <input type="text" v-model="word" :class="currentInputWrong ? 'danger' : ''" />
  </div>
</template>

<script lang="ts" setup>
const word = ref("");
const currentInputWrong = ref(false)
const onNextWord = () => {
  word.value = "";
};

const onWrong = () => {
  currentInputWrong.value = true
}

const onSucess = () => {
  currentInputWrong.value = false
}
</script>

<style scoped>
input.danger {
  background-color: brown;
}
</style>
