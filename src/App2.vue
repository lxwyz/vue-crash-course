
<script setup>
//composition API form.
import { ref , onMounted } from 'vue';
    // optional API form.
    // data(){
    //   return {
    //     name: 'Liam',
    //     status: 'active',
    //     tasks: ['Task One','Task Two','Task Three'],
    //     link: 'https://google.com'
    //   };
    // },
    // methods:{
    //   toggleStatus(){
    //     if(this.status === 'active'){
    //       this.status = 'pending';
    //     }else if(this.status ==='pending'){
    //       this.status = 'inactive';
    //     }else{
    //       this.status = 'active';
    //     }
    //   }
    // }
    
      const name = ref('Liam');
      const status = ref('active');
      const tasks = ref(['Task One','Task Two','Task Three']);
      const newTask = ref('');
      const toggleStatus = () =>{
        if(status.value === 'active'){
          status.value = 'pending';
        } else if(status.value === 'pending'){
          status.value = 'inactive';
        } else{
          status.value = 'active';
        }
      };
   const addTask = () =>{
    if(newTask.value.trim() !== ''){
       tasks.value.push(newTask.value);
       newTask.value = '';
    }
   };
   const deleteTask = (index) => {
    tasks.value.splice(index,1);
   };
 
  onMounted(async () =>{
    try {
      const response = await fetch('https://jsonplaceholder.typicode.com/todos');
      const data = await response.json();
      tasks.value = data.map((task)=>task.title);
    } catch (error) {
      console.log('Error fetching data');
    }
  });
</script>

<template>
   <h1> {{name}}</h1>
   <p v-if="status ==='active'">User is Active</p>
   <p v-else-if="status === 'pending'">User is Pending</p>
   <p v-else>User is inactive</p>

   <form @submit.prevent="addTask">
    <label for="newTask">Add Task</label>
    <input type="text" id="newTask" name="newTask" v-model="newTask">
    <button type="submit">Submit</button>
   </form>
   <h3> Tasks:</h3>
  <ul>
    <li v-for="(task,index) in tasks" :key="task">
      <span>{{ task }}</span>
      <button @click="deleteTask(index)">X</button>
    </li>
  </ul>
  <!-- <a :href="link">Click for Google</a> -->
  <br>
  <button @click="toggleStatus">Change Status</button>
</template>
<!-- 
<style scoped>
  h1 {
    color: red;
  }
</style> -->
