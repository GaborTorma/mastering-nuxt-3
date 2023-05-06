<template>
  <label
    class="rounded text-white font-bold py-2 px-4 cursor-pointer"
    :class="{
      'bg-green-500': isLessonComplete,
      'bg-gray-500': !isLessonComplete,
    }"
  >
    <input
      type="checkbox"
      :value="isLessonComplete"
      @input="toggleComplete"
      class="hidden"
    />
    {{ text }}
  </label>
</template>

<script setup>
const props = defineProps({
  chapterNumber: {
    type: Number,
    required: true,
  },
  lessonNumber: {
    type: Number,
    required: true,
  },
});

const progress = useLocalStorage('progress', []);

const isLessonComplete = computed(() => 
  progress.value[props.chapterNumber - 1]?.[props.lessonNumber - 1]
);

const text = computed(()=> 
  isLessonComplete ? 'Completed!' : 'Mark as complete'
)

const toggleComplete = () => {
  if (!progress.value[props.chapterNumber - 1]) {
    progress.value[props.chapterNumber - 1] = [];
  }

  progress.value[props.chapterNumber- 1][
    props.lessonNumber - 1
  ] = !isLessonComplete.value;
};
</script>

<style scoped>
::selection {
  display: none;
}
</style>
