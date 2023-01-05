<template>

  <p class="itemId tracking-normal">{{ index + 1 }}.</p>
  <div class="itemTitleWrap ml-2 overflow-x-auto">
    <p>{{ props.title }}</p>
  </div>
  <!-- <div class="active w-[16px] h-[16px] bg-white absolute rotate-45 top-[20%] right-[-8px] z-[1]"
    :class="{ 'hidden': currentNum !== index }"></div> -->
  <div class=" absolute rotate-180 right-0 text-white text-2xl top-[-4px]" :class="{ 'hidden': currentNum !== index }">
    <i class="fa-solid fa-play"></i></div>
</template>

<script setup>

const props = defineProps({
  sid: Number,
  title: String,
  currentNum: Number,
  index: Number
});

// console.log(props.content);

</script>

<style lang="scss" scoped>

</style>