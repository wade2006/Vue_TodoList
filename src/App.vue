<template>
  <div class="todo-container">
    <div class="todo-wrap">
      <TodoHeader :addTodo="addTodo"/>
      <TodoList :todos="todos" :deleteTodo="deleteTodo"/>
      <TodoFooter :todos="todos" :deleteCompleteTodos="deleteCompleteTodos" :selectAllTodos="selectAllTodos" />
     </div>
  </div>
</template>

<script>
import TodoHeader from './components/TodoHeader'
import TodoFooter from './components/TodoFooter'
import TodoItem from './components/TodoItem'
import TodoList from './components/TodoList'
export default {
  data(){
  return {
    //从localStorage读取todos
    todos:JSON.parse(window.localStorage.getItem('todos_key')||'[]')//JSON.parse()从一个字符串中解析出json对象
  }
  },
  watch:{  //监视
    todos:{
      deep:true,  //深度监视
      handler:function(newvalue){
       //将todos最新的值的json数据保存到localStorage
       window.localStorage.setItem('todos_key',JSON.stringify(newvalue))//JSON.stringify()从一个对象中解析成字符串
      }

    }

  },
  methods:{
  addTodo(todo){
    this.todos.push(todo)
  },
  deleteTodo(index){
    this.todos.splice(index,1)
  },
  deleteCompleteTodos(){  //删除所有完成的todo
   this.todos= this.todos.filter(todo=>!todo.iscompleted)  //过滤掉iscompleted为true的item
  },
  selectAllTodos(ischeck){  //全选/全不选
   this.todos.forEach(todo=>todo.iscompleted=ischeck)
  }
  },
 components:{
   TodoHeader,
   TodoList,
   TodoFooter
   }
}
</script>

<style>
/*app*/
.todo-container {
  width: 600px;
  margin: 0 auto;
}
.todo-container .todo-wrap {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
}
</style>