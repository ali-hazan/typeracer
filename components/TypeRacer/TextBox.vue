<template>
  <div class="text-box">
    <span class="history">{{ history }}</span>
    <span class="current-letter"></span>
    <span class="current-word">{{ currentWord }}</span>
    <span class="remaining">{{ remaining }}</span>
  </div>
</template>

<script lang="ts" setup>
const props = defineProps({
  word: {
    type: String,
    default: "",
  },
});

const emit = defineEmits(["next-word", "game-over"]);

const { word } = toRefs(props);

const text = `Hello world this is type racer game`;

const currentWordIndex = ref(0);
const currentLetterIndex = ref(0);

const resetGame = () => {
  currentWordIndex.value = 0;
  currentLetterIndex.value = 0;
};

const textArray = computed(() => {
  return text.split(" ");
});

const history = computed(() => {
  return textArray.value.slice(0, currentWordIndex.value).join(" ");
});

const currentWord = computed(() => {
  return " " + textArray.value[currentWordIndex.value] + " ";
});

const remaining = computed(() => {
  return textArray.value.slice(currentWordIndex.value + 1).join(" ");
});

const onNextWord = () => {
  if (textArray.value.length - 1 > currentWordIndex.value) {
    currentWordIndex.value = currentWordIndex.value + 1;
    currentLetterIndex.value = -1;
  } else {
    emit("game-over");
    resetGame();
  }
};

const onPrevWord = () => {
  if (currentWordIndex.value > 0) {
    currentWordIndex.value = currentWordIndex.value - 1;
    currentLetterIndex.value = currentWord.value.replace(/ +/g, "").length - 1;
  }
};

const onNextCharacter = () => {
  if (
    currentWord.value.replace(/ +/g, "").length - 2 >
    currentLetterIndex.value
  ) {
    currentLetterIndex.value = currentLetterIndex.value + 1;
  } else {
    onNextWord();
  }
};
const onPrevCharacter = () => {
  if (currentLetterIndex.value > 0) {
    currentLetterIndex.value = currentLetterIndex.value - 1;
  } else {
    onPrevWord();
  }
};

watch(word, (val) => {
  if (val === currentWord.value.replace(/ +/g, "")) {
    onNextWord();
    emit("next-word");
  }
});
</script>

<style lang="css">
.text-box {
  background-color: #fafafa;
  padding: 12px;
  font-size: 28px;
}
.history {
  background-color: white;
}
.current-word {
  background-color: blue;
}
.remaining {
  background-color: red;
}
</style>
