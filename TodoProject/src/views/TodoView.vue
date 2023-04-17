

<template>
  <div>
    <h2 style=" text-align: center; margin-bottom: 20px ;">Ajouter une tâche :</h2>
    <div>
      <label style="margin-right: 10px; " for="name">Nom :</label>
      <input id="name" type="text" v-model="name">
      
      <label style="margin-left: 10px;margin-right: 10px; " for="hours">Heures :</label>
      <input id="hours" type="number" v-model="hours"> <br>
      <label style="margin-right: 10px;" for="manager">Responsable : </label>
      <select id="manager" v-model="manager">
        <option value="">Sélectionnez un nom</option>
        <option v-for="name in names" :value="name">{{ name }}</option>
      </select>
      <button style="margin-left: 10px;margin-right: 10px; " @click="addTask">Ajouter</button>
    </div>
    <hr>
    <h2>Liste des tâches :</h2>
    <ul>
      <li v-for="(task, index) in tasks" :key="index" :class="{'completed': task.completed }">
        {{ task.name }} - {{ task.hours }} heures - {{ task.randomName }}
        <button style="margin-left: 10px;margin-right: 10px; " @click="TaskDone(index)">Terminé</button>
        <button style="margin-left: 10px;margin-right: 10px; "  @click="editTask(index)">Modifier</button>
        <button style="margin-left: 10px;margin-right: 10px; "  @click="deleteTask(index)">Supprimer</button>
      </li>
    </ul>
    <div>
      <p>Tâches en cours : {{ ongoingTasks }}</p>
  <p>Tâches terminées : {{ completedTasks }}</p>
  <p>Total de Tâches créées : {{ totalTasks }}</p>
</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      name: '',
      hours: '',
      manager: '',
      names: ['Antoine', 'Maeva', 'Victor', 'Wilhem', 'Romain'],
      totalTasks : 0,
      completedTasks : 0,
      ongoingTasks : 0,
      tasks: []
    
    }
  },
  methods: {
    addTask() {
  if (this.name !== '' && this.hours !== '' && this.manager !== '') {
    this.tasks.push({
      name: this.name,
      hours: this.hours,
      manager: this.manager,
      completed: false
      
    }
    );
  
    this.name = '';
    this.hours = '';
    this.manager = '';
    this.ongoingTasks++
    this.totalTasks = this.tasks.length; // initialisation de totalTasks
    this.completedTasks = this.task.completed.length
    this.ongoingTasks =  this.tasks.length; // this.tasks.filter(task => !task.completed).length; // initialisation de ongoingTasks
  }

},

TaskDone(index) {
  this.tasks[index].completed = true;
  this.completedTasks++;
  this.ongoingTasks--;
},
deleteTask(index) {
  if (this.tasks[index].completed) {
    this.completedTasks--;
  } else {
    this.ongoingTasks--;
  }
  this.tasks.splice(index, 1);
  this.totalTasks = this.tasks.length;
},
  }
  }
</script>

<style>

.completed {
  border: 2px solid green;
}


</style>

<style>
@media (min-width: 1024px) {
  .about {
    min-height: 100vh;
    display: flex;
    align-items: center;
  }
}
</style>
