<script setup>
import { ref, reactive, watch } from 'vue';
import TodoItem from './components/TodoItem.vue';

const now = new Date();
const tomorrow = new Date(now.getTime() + (24 * 60 * 60 * 1000));
// console.log(tomorrow);
const nowYear = now.getFullYear();
const nowMonth = `${now.getMonth() + 1}`.padStart(2, '0');
const nowDate = `${now.getDate()}`.padStart(2, '0');
const defaultStartDate = nowYear + '-' + nowMonth + '-' + nowDate;
const tomYear = tomorrow.getFullYear();
const tomMonth = `${tomorrow.getMonth() + 1}`.padStart(2, '0');
const tomDate = `${tomorrow.getDate()}`.padStart(2, '0');
const defaultEndDate = tomYear + '-' + tomMonth + '-' + tomDate;
// console.log(tomYear + '-' + tomMonth + '-' + tomDate);
const todoItems = reactive(JSON.parse(localStorage.getItem('todo')) && JSON.parse(localStorage.getItem('todo')).length > 0 ? JSON.parse(localStorage.getItem('todo')) : JSON.parse(JSON.stringify([{ "id": 1, "title": "item title1", "content": "content1...", "image": "/", "startDate": defaultStartDate, "endDate": defaultEndDate, }])))

const currentNum = ref(0);
const show = ref(false);

const handleClick = (sid) => {
  currentNum.value = sid;
};

const handleAdd = () => {
  todoItems.push({
    id: todoItems[todoItems.length - 1].id + 1,
    title: 'new item title',
    content: 'content...',
    image: '/',
    startDate: defaultStartDate,
    endDate: defaultEndDate,
  });
  currentNum.value = todoItems.length - 1;
};

const imageLoaded = (e) => {
  todoItems[currentNum.value].image = e.target.result;
}

const handleChange = (e) => {
  const file = e.target.files.item(0);
  const reader = new FileReader();
  reader.addEventListener('load', imageLoaded);
  reader.readAsDataURL(file);
}

const handleRemove = (item) => {
  if (todoItems.length > 1) {
    const index = todoItems.indexOf(item);
    todoItems.splice(index, 1);
    currentNum.value = 0;
    // console.log({ index, item });
  }
}

const handleDateSelect = () => {
  if (todoItems[currentNum.value].startDate >= todoItems[currentNum.value].endDate) {
    let dateEnd = new Date(todoItems[currentNum.value].endDate).getTime();
    // console.log(new Date(dateEnd + (24 * 60 * 60 * 1000)));
    const year = new Date(dateEnd - (24 * 60 * 60 * 1000)).getFullYear();
    // 補足 2 位數
    const month = `${new Date(dateEnd - (24 * 60 * 60 * 1000)).getMonth() + 1}`.padStart(2, '0');
    const date = `${new Date(dateEnd - (24 * 60 * 60 * 1000)).getDate()}`.padStart(2, '0');
    // console.log(year + '-' + month + '-' + date);
    todoItems[currentNum.value].startDate = year + '-' + month + '-' + date;
    alert('開始日期不得大於等於結束日期');
  }
}

const handleShow = () => {
  show.value = !show.value;
}

// console.log({ todoItems, currentNum });

watch(todoItems, (newData) => {

  localStorage.setItem('todo', JSON.stringify(newData));
}, { deep: true })
// watch(currentNum, (newData) => {
//   console.log(newData);
// }, { deep: true })

</script>

<template>
  <div class="todoList flex">
    <transition name="ease-out-overlay">
      <div
        class="sidebar md:w-[10%] w-[80%] z-10 h-screen bg-[#A1FFc7] md:block md:static absolute md:translate-x-0 transition-transform duration-500 md:transition-none"
        :class="{
  'show': show
}">
        <div class="titleWrap ml-4 flex mt-1">
          <p class=" text-base font-bold">Demo Todo List</p>
          <i class="fa-solid fa-x ml-auto translate-x-[-10px] font-bold mt-1 md:hidden" @click="handleShow"> </i>
        </div>
        <div div class=" todoItems mt-3">
          <!-- components -->
          <div v-for="(item, i) in   todoItems" class="item flex pl-2 mt-1 bg-[#81F8B1] relative"
            @click="handleClick(i)">
            <TodoItem :title="item.title" :index="i" :sid="item.id" key="item.id" :currentNum="currentNum">
            </TodoItem>
          </div>
        </div>
        <div class="addBtn mt-3 flex justify-center" :class="{ hidden: todoItems.length >= 10 }">
          <button class="bg-[#E7FFE9] py-1 px-5 w-[80%] rounded hover:bg-[#81F8B1]" @click="handleAdd">Add Item</button>
        </div>
      </div>
    </transition>
    <div class="w-full h-screen bg-[#0000005b] absolute z-[8]" :class="{ hidden: !show }" @click="handleShow"></div>

    <div class="content w-full flex flex-col p-3">
      <div class="flex md:ml-auto justify-between">
        <div class="menu cursor-pointer md:hidden ml-1 text-lg" @click="handleShow"><i class="fa-solid fa-bars"></i>
        </div>
        <div class="delItem ml-3 cursor-pointer" @click="handleRemove(todoItems[currentNum])"><i
            class="fa-solid fa-trash-can"></i>
        </div>
      </div>
      <div class="form flex flex-wrap">
        <div class="titleInput w-full order-1">
          <input type="text" name="title" class=" w-full rounded-md h-8 bg-[#EBEBEB] mt-3 p-3" placeholder="Title..."
            v-model="todoItems[currentNum].title" />
        </div>
        <div class="flex justify-between md:w-[50%] w-full md:h-[270px] h-[220px] mb-8 flex-wrap md:mb-0  order-2">
          <div class="contentInput w-full md:w-[90%] h-full relative">
            <textarea name="content" id="content" cols="30" rows="10"
              class="w-full h-full bg-[#EBEBEB] my-3 mr-3 p-3 rounded-md" v-model="todoItems[currentNum].content"
              maxlength="200"></textarea>
            <!-- 字數計算 -->
            <span class=" absolute md:right-[2%] md:bottom-[5%] right-[5%] bottom-0">200/{{
    todoItems[currentNum].content.length
}}</span>
          </div>
        </div>
        <div
          class="imageWrap bg-[#EBEBEB] md:my-3 rounded-md  md:w-[50%] h-[269px] w-full mt-5 md:order-3 order-4 md:ml-auto">
          <img :src="todoItems[currentNum].image" alt="請上傳圖片" class="h-full w-full object-cover rounded-md">
        </div>

        <div class="dateInput flex items-center md:w-[48%] w-full md:order-4 order-3 md:justify-start justify-around">
          <input type="date" class="start bg-[#EBEBEB] py-2 md:px-28 rounded-md px-3"
            v-model="todoItems[currentNum].startDate" @change="handleDateSelect" />
          <span class="md:mx-5 mx-2">~</span>
          <input type="date" class="end bg-[#EBEBEB] py-2 md:px-28 rounded-md px-3"
            v-model="todoItems[currentNum].endDate" @change="handleDateSelect" />
        </div>

        <div class="uploadBtn md:w-[50%] w-full md:ml-8 order-5 md:mt-0 mt-5">
          <input type="file" ref="imageUpload" name="imageUpload" id="imageUpload" accept="image/png, image/jpeg, .gif"
            class="hidden" @change="handleChange" />
          <button class="bg-[#E7FFE9] w-full py-2 md:px-36 md:mt-3 rounded-md hover:bg-[#81F8B1]"
            @click="$refs.imageUpload.click()">Upload
            Image</button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>

</style>
