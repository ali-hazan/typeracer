<template>
  <div class="text-box">
    <span class="history">{{ history + ' ' }}</span>
    <span class="current-word">{{ currentWord.slice(0, currentLetterIndex) }}<span class="current-letter">{{
      currentWord[currentLetterIndex] }}</span>{{ currentWord.slice(currentLetterIndex + 1) }}</span>
    <span class="remaining"> {{ ' ' + remaining }}</span>
  </div>
</template>

<script lang="ts" setup>
const props = defineProps({
  text: {
    type: String,
    default: "",
  },
  word: {
    type: String,
    default: "",
  },
});
const emit = defineEmits(["next-word", "game-over", "is-wrong", "is-success", "on-progress"]);
const { word } = toRefs(props);
const currentWordIndex = ref(0);
const currentLetterIndex = ref(0);

const textArray = computed(() => {
  return props.text.split(" ");
});
const history = computed(() => {
  return textArray.value.slice(0, currentWordIndex.value).join(" ");
});
const currentWord = computed(() => {
  return textArray.value[currentWordIndex.value];
});
const remaining = computed(() => {
  return textArray.value.slice(currentWordIndex.value + 1).join(" ");
});

const resetGame = () => {
  currentWordIndex.value = 0;
  currentLetterIndex.value = 0;
};

const calculateProgress = () => {
  const totalWord = textArray.value.length;
  const progress = currentWordIndex.value / (totalWord - 1) * 100
  emit('on-progress', progress)
}

const onNextWord = () => {
  calculateProgress()
  currentLetterIndex.value = 0
  if (textArray.value.length - 1 > currentWordIndex.value) {
    currentWordIndex.value = currentWordIndex.value + 1;
  } else {
    emit("game-over");
    resetGame();
  }
};


const getLeastIndex = (str1: string, str2: string) => {
  if (str1.length <= str2.length) {
    return str1.length
  }
  else {
    return str2.length
  }
}

const calculateCurrentLetterIndex = () => {
  const currentWordWithoutSpace = currentWord.value.replace(/ +/g, "")
  const limit = getLeastIndex(word.value, currentWordWithoutSpace)
  let letterIndex = 0
  let isSuccess = true
  for (let i = 0; i < limit; i++) {
    if (currentWordWithoutSpace[i] === word.value[i]) {
      letterIndex = i
    }
    else {
      isSuccess = false
      break
    }
  }
  isSuccess ? emit('is-success') : emit('is-wrong');

  currentLetterIndex.value = letterIndex + 1
}

watch(word, (val) => {
  if (val.replace(/ +/g, "") === currentWord.value.replace(/ +/g, "")
    && (val[val.length - 1] === ' '
      || textArray.value.length - 1 === currentWordIndex.value)) {
    onNextWord();
    emit("next-word");
    return
  }
  if (val) {
    nextTick(() => {
      calculateCurrentLetterIndex()
    })
  }
  else {
    currentLetterIndex.value = 0
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
  text-decoration: underline;
}

@keyframes border-anim {
  0% {
    border-left: 1px solid #000;
  }

  40% {
    border-left: 1px solid #fff;
  }

  100% {
    border-left: 1px solid #fff;
  }
}

.current-letter {
  position: relative;

}

.current-letter::before {
  content: "";
  position: absolute;
  height: 32px;
  width: 2px;
  animation-name: border-anim;
  animation-duration: 1000ms;
  animation-iteration-count: infinite;
  animation-timing-function: ease-in;
}
</style>
