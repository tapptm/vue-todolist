<template > 

  <div id="todoApp">
  <h3> {{message}} </h3>
  <form name="todo-form" method="post" action="" v-on:submit.prevent= "addTask">
    <input name="add-todo" type="text" v-model= "addTodoInput" v-bind:class= "{error: hasError}"/>
    <button type="submit">Add</button>

    <!-- {{addTodoInput}} -->
  </form>
<h3>My Todo Tasks</h3>
  <div class="todo-lists" v-if="lists.length">
    
    <ul>
      <li v-for="list in filterLists" :key="list.id">
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
     message: 'Welcome to Todo App',
     addTodoInput: ''  , 
     lists: [], // this will hold all the created todo task items
     hasError: false  // <-- to handle errors
   } 
  },

  computed: {
          filterLists: function(){
            return _.orderBy(this.lists, ['isComplete', false])
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
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
