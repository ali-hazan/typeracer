<template>
  <div class="type-racer">
    <TypeRacerTrack :progress="progress" class="margin-bottom" />
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
.type-racer {
  max-width: 784px;
  margin: auto;
  text-align: center;
}

input {
  margin-top: 32px;
  border: 1px solid #000;
  font-size: 18px;
  padding: 4px 8px;
  outline: none;
}

input.danger {
  color: red;
}

.margin-bottom {
  margin-bottom: 48px;
}</style>
