<template>
    <div class="container">
        <h1>Tasks({{ todos.length }})</h1>
        <form @submit.prevent="createTask">
            <div class="input-group mb-3">
                <input type="text" class="form-control" placeholder="New Task" v-model="todo.title" aria-describedby="button-addon2">
                <div class="input-group-append">
                    <button class="btn btn-primary" type="submit" id="button-addon2"><b>+</b> Add</button>
                </div>
            </div>
        </form>
        <ul class="list-group">
            
            <li v-for="todo in todos" :key="todo.id" v-bind:class="{'completed':(todo.is_completed === 1)}" class="list-group-item d-flex justify-content-between align-items-center">
                <span class="stikethrough" @click="updateTodoStatus(todo.id)">{{ todo.title }}</span>
                <span class="badge badge-danger badge-pill" @click="deleteTodoItem(todo.id)">X</span>
            </li>
            
        </ul>
    </div>


</template>
<script>
import axios from 'axios'
export default {
    data() {
            return {
                todos: [],
                todo: {
                    title: ''
                }
            }
        },
    created() {
            this.fetchTodoList();
    },
    methods: {
        createTask() {
            axios
                .post('http://localhost:8000/todo', this.todo)
                .then(() => {
                    this.fetchTodoList();
                    this.todo.title = '';
                })
                
        },
        fetchTodoList() {
            axios
                .get('http://localhost:8000/todo/')
                .then(response => {
                    this.todos = response.data;
                });

        },
        deleteTodoItem(id) { 
            axios
                .delete(`http://localhost:8000/todo/${id}`)
                .then(() => {
                    this.fetchTodoList();
                });
        },
        updateTodoStatus(id) { 
            axios
                .put(`http://localhost:8000/todo/${id}`)
                .then(() => {
                    this.fetchTodoList();
                });
            }
    }
}
</script>
<style scoped>
.completed {
    background-color: #90EE90;
    
}
.completed .stikethrough{
    text-decoration: line-through;
}
.badge-danger {
    background-color:red !important;
    
}
</style>