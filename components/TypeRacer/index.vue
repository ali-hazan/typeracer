<template>
  <div>
    <TypeRacerTrack :progress="progress" />
    <TypeRacerTextBox :word="word" :text="text" @next-word="onNextWord" @is-wrong="onWrong" @is-success="onSucess"
      @on-progress="onProgress" />
    <input type="text" v-model="word" :class="currentInputWrong ? 'danger' : ''" />
  </div>
</template>

<script lang="ts" setup>
const word = ref("");
const currentInputWrong = ref(false)
const text = `Hello world this is type racer game`
const progress = ref(0)

const onProgress = (val: number) => {
  progress.value = val
}

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
