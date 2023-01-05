<template>
  <div class="sidebar w-[10%] h-screen bg-[#A1FFc7]">
    <div class="titleWrap ml-4">
      <p class=" text-base font-bold">Demo Todo List</p>
    </div>
    <div class="todoItems mt-3">
      <!-- components -->
      <TodoItem></TodoItem>
      <TodoItem></TodoItem>
      <!-- <div class="item flex pl-3 mt-1 bg-[#81F8B1] relative">
        <p class="itemId tracking-wide">1.</p>
        <div class="itemTitleWrap ml-2">
          item title
        </div>
        <div class="active w-[16px] h-[16px] bg-white absolute rotate-45 top-[20%] right-[-8px]"></div>
      </div>
      <div class="item flex pl-3 mt-1 bg-[#81F8B1]">
        <p class="itemId tracking-wide">1.</p>
        <div class="itemTitleWrap ml-2">
          item title
        </div>
      </div> -->
    </div>
    <div class="addBtn mt-3 flex justify-center">
      <button class="bg-[#E7FFE9] py-1 px-5 w-[80%] rounded hover:bg-[#81F8B1]">Add Item</button>
    </div>
  </div>

  <div class="content">
    <div class="delItem"><i class="fa-solid fa-trash-can"></i></div>
  </div>
</template>

<script setup>
import TodoItem from './components/TodoItem.vue';


</script>

<style scoped>

</style>
