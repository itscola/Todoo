<template>
    <div class="main">
        <div class="container">
            <div class="header">
                <h1 class="title">待办事项</h1>
                <hr style="width: 100%; margin-top: 10px; margin-bottom: 10px;color: darkgray;">

                <div class="input">
                    <input type="text" placeholder="请输入待办事项" v-model="todo" @keyup.enter="addTodo" />
                </div>
            </div>




            <div class="list">
                <div class="todo" v-for="(item, index) in filteredTodos" :key="index">
                    <input class="checkbox1" type="checkbox" :checked="item.completed"
                        @change="toggleCompleted(index)" />
                    <span class="list-text" :class="{ completed: item.completed }" @click="toggleCompleted(index)">{{
                        item.text }}</span>
                    <img src="../assets/x.png" alt="删除" @click="removeTodo(index)" class="delete-button" />
                </div>
            </div>
            <div class="info">
                <span>共 {{ uncompletedCount }} 条未完成任务</span>
                <div class="options">
                    <button @click="setFilter('all')">All</button>
                    <button @click="setFilter('active')">Active</button>
                    <button @click="setFilter('completed')">Completed</button>
                </div>
            </div>
        </div>


    </div>
</template>

<script setup>

import { ref, computed } from 'vue'

const todo = ref('')
const todos = ref([
    { text: '学习 Vue 3', completed: false },
    { text: '学习 TypeScript', completed: false },
    { text: '学习 Vite', completed: false }
])
const addTodo = () => {
    if (todo.value.trim() === '') return
    todos.value.push({ text: todo.value, completed: false })
    todo.value = ''
}
const removeTodo = (index) => {
    todos.value.splice(index, 1)
}
const toggleCompleted = (index) => {
    todos.value[index].completed = !todos.value[index].completed
}
const clearCompleted = () => {
    todos.value = todos.value.filter(todo => !todo.completed)
}

const uncompletedCount = computed(() => {
    return todos.value.filter(todo => !todo.completed).length;
});

const filter = ref('all');

const filteredTodos = computed(() => {
    if (filter.value === 'active') {
        return todos.value.filter(todo => !todo.completed);
    } else if (filter.value === 'completed') {
        return todos.value.filter(todo => todo.completed);
    }
    return todos.value;
});

const setFilter = (newFilter) => {
    filter.value = newFilter;
};

</script>

<style scoped>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

.main {
    width: 100%;
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
}

.container {
    display: flex;
    flex-direction: column;
    background-color: #f1f1f1;
    border-radius: 8px;
    width: 400px;
    align-items: center;

}

.header {
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 100%;
    padding: 20px;
    padding-bottom: 0px;
    /* box-shadow: -5px 5px 10px rgba(0, 0, 0, 0.1); */
}


.title {
    font-size: 40px;
    font-family: Arial, Helvetica, sans-serif;
}

.input {
    width: 100%;
}

.input input {
    margin-top: 5px;
    ;
    width: 100%;
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 4px;
    font-size: 16px;
}

.list {
    width: 100%;
}

.todo {
    width: 100%;
    display: flex;
    /* 添加 flex 布局 */
    align-items: center;
    /* 添加垂直居中对齐 */
    border-bottom: 1px solid #ccc;
    padding-left: 23px;
    padding-top: 16px;
    padding-bottom: 16px;

}

.todo:hover .delete-button {
    opacity: 1;
    /* 悬停时显示 */
}

.checkbox1 {
    appearance: none;
    width: 30px;
    height: 30px;
    border: 1px solid #ccc;
    border-radius: 50%;
    vertical-align: middle;
    margin-right: 10px;
    cursor: pointer;
    outline: none;
    /* 移除默认 outline */
}

/* 确保 focus 状态下也没有 outline 或 shadow */
.checkbox1:focus,
.checkbox1:checked:focus {
    outline: none;
    box-shadow: none;
}

.checkbox1:checked {
    /* 再次微调 SVG transform 和 background-size */
    background-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 50 50"><path fill="%235dc2af" d="M72 25L42 71 27 56l-4 4 20 20 34-52z" transform="translate(-22 -18) scale(0.85)"/></svg>');
    background-size: 55% 55%;
    /* 稍微调整背景大小 */
    background-repeat: no-repeat;
    /* 确保不重复 */
    background-position: center;
    /* 确保居中 */
    border-color: #bddad5;
}

.list-text {
    font-size: 20px;
    color: #333;
    cursor: pointer;
    font-family: sans-serif;
    margin-left: 5px;
}

.list-text.completed {
    text-decoration: line-through;
    color: #ccc;
    /* 添加灰色字体颜色 */
}

.delete-button {
    width: 20px;
    height: 20px;
    margin-left: auto;
    margin-right: 22px;
    cursor: pointer;
    opacity: 0;
    /* 初始状态隐藏 */
    transition: opacity 0.3s ease;
    /* 添加过渡效果 */
}

.info {
    font-family: sans-serif;
    display: flex;
    justify-content: space-between;
    align-items: center;
    width: 100%;
    margin: 10px;
    padding: 10px 20px;
}

.options button {
    padding: 8px 12px;
    margin-left: 5px;
    cursor: pointer;
    border: 1px solid transparent;
    border-radius: 3px;
    color: inherit;
}
</style>