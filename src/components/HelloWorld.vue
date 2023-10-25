<template>
  <input type="text" v-model="todoMsg">
  <button @click="add">添加</button>
  <button @click="clearHasDone">清除</button>
  <div v-if="lists.length">
    <div v-for="(item,index) in lists" :key="item.msg">
      <input type="checkbox" v-model="item.done">
      <span :class="{done: item.done}">{{item.msg}}</span>
      <span @click="deleteItem(index)">❎</span>
    </div>
    <div>
      <span>全选</span>
      <input type="checkbox" v-model="isAllDone">
      <span>{{hasDone}}/{{ lists.length }}</span>
    </div>
  </div>
  <div v-else>暂无数据</div>
</template>

<script lang="ts">
import { computed, defineComponent, ref } from 'vue'

export default defineComponent({
  setup() {
    interface todoItem{
      msg: string,
      done:boolean,
    };

    const todoMsg = ref<string>('');

    const lists = ref<todoItem[]>([
      { msg: '吃饭', done: true },
      { msg: '睡觉', done: false },
      { msg: '打游戏', done: false },
    ]);

    const hasDone = computed(() => lists.value.filter(item => item.done).length);

    const isAllDone = computed<boolean>({
      get() {
        return hasDone.value === lists.value.length
      },
      set(value: boolean) {
        lists.value.forEach(item => {
          item.done = value
        })
      }
    })

    const add = () => {
      if (todoMsg.value) {
        lists.value.push({
          msg: todoMsg.value,
          done: false
        })
        todoMsg.value = ''
      }
    }

    const deleteItem = (index: number) => {
      lists.value.splice(index, 1)
    }

    const clearHasDone = () => {
      lists.value=lists.value.filter(item=>!item.done)
    }

    return {
      todoMsg,
      lists,
      hasDone,
      isAllDone,
      add,
      deleteItem,
      clearHasDone
    }
  },
})
</script>


<style>
  #app{
    margin: 20px;
    padding: 0;
    text-align: start;
  }
  button{
    margin: 0 10px;
  }
  .done{
    text-decoration: line-through;
    color: gray;
  }
</style>