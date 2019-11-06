<template > 

  <div id="todoApp">
  <h3> {{message}} </h3>
  <form name="todo-form" method="post" action="" v-on:submit.prevent= "addTask">
    <input  type="text" v-model= "addTodoInput" v-bind:class= "{error: hasError}"/>
    <button type="submit">Add</button>
    <!-- {{addTodoInput}} -->
  </form>
<h3>New ToDo</h3>
  <div class="todo-lists" v-if="lists.length">
    <ul>
      <li v-for="list in lists" :key="list.id">
        <input type="checkbox" v-on:change="completeTask(list)" v-bind:checked="list.isComplete"/>
        <span class="title" 
              contenteditable="true" 
              v-on:keydown.enter="updateTask($event, list)" 
              v-on:blur="updateTask($event, list)" 
              v-bind:class="{completed: list.isComplete}">{{list.title}}
        </span> 
        <span class="remove" v-on:click="removeTask(list)">x</span>
      </li>
    </ul>
  </div>
</div>

</template>

<script>

import _ from 'lodash'

 export default {
  name: 'todoapp',
  
  data : function () {
   return {
     message: 'New ToDo',
     addTodoInput: ''  , 
     lists: [], // this will hold all the created todo task items
     hasError: false  // <-- to handle errors
   } 
  },

  methods:{
    addTask: function(){  //form submit action goes here
      if(!this.addTodoInput){  // <--- If no value then we are setting error to `true`
        this.hasError = true;
        return;                // <--- stops here
      }
        this.hasError = false; // <--- If textbox is filled then setting error to `false`
          
      this.lists.push({
        id: this.lists.length+1,
        title: this.addTodoInput,
        isComplete: false
      });
      this.addTodoInput = ''; //clear the input after successful submission
     
    },
    updateTask: function(e, list){
      e.preventDefault();
      list.title = e.target.innerText;
      e.target.blur();
    },
    completeTask: function(list){
      list.isComplete = !list.isComplete;
    },
    removeTask: function(list){
      var index = _.findIndex(this.lists, list);
      this.lists.splice(index, 1);
    }
  }
  
}
    
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

input[type=text].error{border: 1px solid red;}

.title{
  display: inline-block;
  width: 200px;
  border: 1px solid transparent;
  padding: 8px;
  font-size: 16px;
  vertical-align:middle;
}

.remove{
  cursor:pointer;
  display:inline-block;
  border: 1px solid #c4c4c4;
  border-radius: 50%;
  padding:0px 4px;
}




</style>
