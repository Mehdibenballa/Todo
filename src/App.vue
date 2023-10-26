<script setup>
import { useDark, useToggle } from '@vueuse/core'
import draggable from 'vuedraggable'

import lightmode from '@/components/icons/lightmode.vue'
import darkmode from '@/components/icons/darkmode.vue';
import deleteb from '@/components/icons/deleteb.vue'
import check from '@/components/icons/check.vue'
import { ref } from 'vue';

const inputVal = ref('')
const drag = ref(false)
const activeTab = ref(1)



const arr = ref([])
const filtredArr = ref([])

const removeTask = (index) => {
  arr.value.splice(index,1)
  filtredArr.value.splice(index,1)
}
const clearComplete = () => {
  arr.value = arr.value.filter((arr) => !arr.completed)
  filtredArr.value = filtredArr.value.filter((arr) => !arr.completed)
};

const all = () => {
  activeTab.value = 1
  filtredArr.value = arr.value;
}
const completed = () => {
  activeTab.value = 3
  filtredArr.value = arr.value.filter(arr => arr.completed);
};
const  active = () => {
  activeTab.value = 2
  filtredArr.value = arr.value.filter(arr => !arr.completed);
}

const pushFunction = () => {
  if (!inputVal.value) return;
  const val = {
    id: arr.value.length > 0 ? arr.value.length : 0,
    title: inputVal.value,
    completed: false,
  }
  arr.value.push(val); 
  filtredArr.value.push(val); 
  inputVal.value = "";
}


const toggleCompletion = (id) => {
  const index = filtredArr.value.findIndex((item) => item.id === id)
  filtredArr.value[index].completed = !filtredArr.value[index].completed
 
}

const isDark = useDark()
const toggleDark = useToggle(isDark)


</script>

<template>
 <section class="bg-light dark:bg-dark bg-no-repeat bg-cover px-4 pb-24">
  <div class="container lg:w-1/2 pt-16">
    <div class="container flex justify-between items-center pb-12">
      <h1 class="font-bold text-5xl text-light-VeryLightGray">TODO</h1>
      <button @click="toggleDark()">
        <darkmode v-if="isDark" class="text-dark-VeryDarkBlue" />
        <lightmode v-else class="text-light-VeryLightGray" />
      </button>
    </div>
    <div class="bg-light-VeryLightGray dark:bg-dark-VeryDarkDesaturatedBlue lg:p-6 p-4 flex gap-6 rounded-lg">
      <div class="bg-light-VeryLightGray dark:bg-dark-VeryDarkDesaturatedBlue border-2 border-light-LightGrayishBlue dark:border-dark-VeryDarkGrayishBlue w-6 h-6 rounded-full px-1">
      </div>
      <input v-model="inputVal" @keyup.enter="pushFunction" placeholder="Creat a new todo" class="outline-none w-full bg-light-VeryLightGray dark:bg-dark-VeryDarkDesaturatedBlue text-lg lg:text-2xl text-dark-VeryDarkDesaturatedBlue dark:text-light-VeryLightGrayishBlue" type="text" name="" id="">
    </div>
  </div>
 </section>
 <section class=" bg-light-VeryLightGrayishBlue dark:bg-dark-VeryDarkBlue px-4">
  <div class="relative container lg:w-1/2 pt-6 pb-6">
    <div class="relative -top-20 ">
      <div class="overflow-y-auto max-h-[400px]">
        <draggable v-model="filtredArr" @start="drag=true" @end="drag=false" item-key="id">
          <template #item="{element}">
            <div class="cursor-move w-full bg-light-VeryLightGray dark:bg-dark-VeryDarkDesaturatedBlue p-4 lg:p-6 flex items-center justify-between gap-6 first:rounded-t-lg border-b border-light-VeryLightGrayishBlue dark:border-dark-VeryDarkGrayishBlueM">
              <div class=" flex gap-8">
                <button 
                  @click="toggleCompletion(element.id)"
                  class="border-2 border-light-LightGrayishBlue dark:border-dark-VeryDarkGrayishBlue w-6 h-6 rounded-full px-1 p-1"
                  :class="element.completed ? 'bg-gradient-to-r from-gradientFrom to-gradientTo' : 'bg-light-VeryLightGray dark:bg-dark-VeryDarkDesaturatedBlue'"
                >
                <check v-if="element.completed"/>
                </button>
                <h2 @click="toggleCompletion(element.id)" :class= "{'line-through text-light-DarkGrayishBlue dark:text-light-DarkGrayishBlue': element.completed}" class="cursor-pointer bg-light-VeryLightGray dark:bg-dark-VeryDarkDesaturatedBlue text-lg lg:text-2xl text-dark-VeryDarkDesaturatedBlue dark:text-light-VeryLightGrayishBlue">
                {{element.title}} 
              </h2>
              </div>
              <button @click="removeTask(index)">
                <deleteb class="font-bold"/>
              </button>
            </div>
          </template>
        </draggable>
      </div>
      <div class="bg-light-VeryLightGray dark:bg-dark-VeryDarkDesaturatedBlue p-4 lg:p-6 flex items-center justify-between gap-6 rounded-b-md">
          <h2 class="text-light-DarkGrayishBlue">
            <span>{{ filtredArr.length }}</span> items left
          </h2>
          <div class="hidden lg:flex gap-6">
            <button @click="all" :class="[activeTab === 1 ? 'text-primary ' :'text-light-DarkGrayishBlue ']" class="outline-none bg-light-VeryLightGray dark:bg-dark-VeryDarkDesaturatedBlue  ">
              All
            </button>
            <button  @click="active" :class="[activeTab === 2 ? 'text-primary ' :'text-light-DarkGrayishBlue']" class="outline-none bg-light-VeryLightGray dark:bg-dark-VeryDarkDesaturatedBlue">
              Active
            </button>
            <button  @click="completed" :class="[activeTab === 3 ? 'text-primary ' :'text-light-DarkGrayishBlue ']" class="outline-none bg-light-VeryLightGray dark:bg-dark-VeryDarkDesaturatedBlue  ">
              Completed
            </button>
          </div>
          <button @click="clearComplete"  class="text-light-DarkGrayishBlue">
            Clear completed
          </button>
      </div>
      <div class=" mt-6 p-4 lg:hidden flex items-center justify-center gap-6 bg-light-VeryLightGray dark:bg-dark-VeryDarkDesaturatedBlue rounded-md">
                <button @click="all" :class="[activeTab === 1 ? 'text-primary ' :'text-light-DarkGrayishBlue ']" class="outline-none bg-light-VeryLightGray dark:bg-dark-VeryDarkDesaturatedBlue  ">
                  All
                </button>
                <button  @click="active" :class="[activeTab === 2 ? 'text-primary ' :'text-light-DarkGrayishBlue']" class="outline-none bg-light-VeryLightGray dark:bg-dark-VeryDarkDesaturatedBlue">
                  Active
                </button>
                <button  @click="completed" :class="[activeTab === 3 ? 'text-primary ' :'text-light-DarkGrayishBlue ']" class="outline-none bg-light-VeryLightGray dark:bg-dark-VeryDarkDesaturatedBlue  ">
                  Completed
                </button>
      </div>
    </div>
  </div>
 </section>
 <footer class="bg-light-VeryLightGrayishBlue dark:bg-dark-VeryDarkBlue flex justify-center items-center">
  <h2 class="text-light-DarkGrayishBlue">Drag and drop to recorder list</h2>
 </footer>    
</template>

<style scoped>

</style>
