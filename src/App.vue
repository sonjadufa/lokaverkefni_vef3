<template>
  <div id="app">
    <div id="hofud">
      <h1 id="AddYourTask">Verkefni</h1>
      <input id="taskInput" class="text" type="text" v-model='task' v-on:keyup.enter='AddTask' placeholder="Hvað þarf að gera?">
      <button id="AddTaskButton" @click='AddTask'>Bæta við</button>
    </div>
    <div id="Tasks" v-for='task in tasks' class="roundedTwo">
      <ul>
        <li>
          <input  type="checkbox" id="roundedTwo" name="check" @click='taskStatus(task.id)' :checked="(task.completed)">
          <label for="roundedTwo" class="text">{{ task.title }} <label id="created">{{task.created}}</label></label>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'app',
  data () {
    return {
      tasks: [],
      task: ''
    };
  },
  
  mounted(){
    var self = this;
    axios.get('http://fjolbraut.org/api/tasks', {
      params: {
        api_token: 'G0h6P9g4NcH94VsGg8CHTeqfJYH1YikmZwXHbJHXmBuogVaNxQdT6ZKlyOwc'}
      })
    .then(function(response) {
      self.tasks = response.data;
      console.log(response)
    })
      .catch(function(error) {
        console.log(error);
      });
    
  },
  methods: {
    AddTask: function(){
      axios.post('http://fjolbraut.org/api/tasks?api_token=G0h6P9g4NcH94VsGg8CHTeqfJYH1YikmZwXHbJHXmBuogVaNxQdT6ZKlyOwc', {
            title: this.task
         })
         .then(function(response) {
          window.location.reload();
         })
         .catch(function(error) {
            console.log(error);
         });
    },
    taskStatus(id){
      axios.post('http://fjolbraut.org/api/tasks/' + id + '/status?api_token=G0h6P9g4NcH94VsGg8CHTeqfJYH1YikmZwXHbJHXmBuogVaNxQdT6ZKlyOwc')
      .then(function(response) {
        window.location.assign()
        })
        .catch(function(error) {
            console.log(error);
        });
      }
    }
  }
</script>

<style>

  html{
    background: #3A1C71;  /* fallback for old browsers */
    background: -webkit-linear-gradient(to top, #FFAF7B, #D76D77, #3A1C71);  /* Chrome 10-25, Safari 5.1-6 */
    background: linear-gradient(to top, #FFAF7B, #D76D77, #3A1C71); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
    height: 100%;
    overflow: hidden;
  }

  body{
    padding-left: 30%;
    padding-right: 30%;
    margin: auto;
  }

  li {
    justify-content: space-between;
    list-style-type: none;
    padding: 10px;
    border-bottom: 1px solid #efefef;
    background-color: pink;
    margin-left: -2.5em;
    border-radius: 5px;
}

  #created{
  float: right;
}

  #taskInput{
    width: 87%;
    height: 1.9em;
    border-radius: 3px;
    border: 3px solid pink;
  }

  #taskInput:hover{
    border-color: hotpink;
  }

  button:hover{
    border-color: hotpink;
  }

  li:hover{
    background-color: hotpink;
  }

  button{
    height: 2.5em;
    border-radius: 3px;
    background-color: white;
    color: black;
    border: 3px solid pink; 
    float: right;
  }

  h1{
    text-align: center;
    border-bottom: dashed;
    border-radius: 8px;
    font-size: 3.5em;
    color: pink;
  }

  input[type=checkbox]:checked + label.text{
    text-decoration: line-through;
    color: grey;
  }

  input.text:focus {
      outline-width: 0;
  }

  </style>