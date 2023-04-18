<template>
  <div style="border: 2px solid green; border-radius: 20px ; padding: 10px;">
    <h2 style=" text-align: center; margin-bottom: 20px ;">Ajouter une tâche :</h2>
    <div>
      <label style="margin-right: 10px; " for="name">Nom :</label>
      <input id="name" type="text" v-model="name">

      <label style="margin-left: 10px;margin-right: 10px; " for="hours">Heures :</label>
      <input id="hours" type="number" v-model="hours"> <br> <br>
      <label style="margin-right: 10px;" for="manager">Responsable : </label>
      <select id="manager" v-model="manager">
        <option value="">Sélectionnez un nom</option>
        <option v-for="name in names" :value="name">{{ name }}</option>
      </select>
      <button style="margin-left: 10px;margin-right: 10px; " @click="addTask">Valider</button>
      <br> <br>
      <p style="color: red">{{ errorHour }}</p> <!-- Affichage du message d'erreur -->
     
    </div>
    <hr>
    <h2 style=" text-align: center; margin-bottom: 20px ;">Liste des tâches :</h2>
    <ul style="margin-bottom: 20px;">
      <li v-for="(task, index) in tasks" :key="index" :class="{ 'completed': task.completed }">
        {{ task.name }} - {{ task.hours }} heures - {{ task.manager }}
        <button style="margin-left: 10px;margin-right: 10px; " @click="TaskDone(index)">Terminé</button>
        <button style="margin-left: 10px;margin-right: 10px; " @click="editTask(index)">Modifier</button>
        <button style="margin-left: 10px;margin-right: 10px; " @click="deleteTask(index)">Supprimer</button>
      </li>
    </ul>
    <hr>
    <h2 style=" text-align: center; margin-bottom: 20px ; margin-top: 20px;">Selection de plusieurs des tâches actives :
    </h2>
    <ul>
      <!-- creer la checkbox pour selectionner plusieurs taches -->
      <li v-for="(task, index) in tasks" :key="index">
        <input type="checkbox" v-model="task.selected">
        {{ task.name }} - {{ task.hours }} heures - {{ task.manager }}

      </li>
     
    </ul>
    <br>
    <button @click="deleteSelectedTasks">Supprimer les tâches sélectionnées</button>
    <div style="text-align: center; margin-top: 100px;">
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
      errorHour: '',
      
      names: ['Antoine', 'Maeva', 'Victor', 'Wilhem', 'Romain'],
      totalTasks: 0,
      completedTasks: 0,
      ongoingTasks: 0,
      tasks: []

    }
  },
  methods: {
    addTask() {
      if (this.name !== '' && this.hours !== '' && this.manager !== '') {
        if (this.hours >= 0) { // Vérification si l'heure est positive
          this.tasks.push({
            name: this.name,
            hours: this.hours,
            manager: this.manager,
            completed: false
          });

          this.name = '';
          this.hours = '';
          this.manager = '';
          this.ongoingTasks++;
          this.totalTasks = this.tasks.length;
          this.completedTasks = this.tasks.filter(task => task.completed).length;
          this.ongoingTasks = this.tasks.filter(task => !task.completed).length;
          this.errorHour = ''; // Réinitialisation du message d'erreur
          this.errorMessage = '';
        } else {
          this.errorHour = 'L\'heure ne peut pas être négative'; // Affichage du message d'erreur
         
        }
      }
    
    },

    deleteSelectedTasks() {
      this.tasks = this.tasks.filter(task => !task.selected);
    },

    TaskDone(index) {
  if (this.tasks[index].completed) {
    this.tasks[index].completed = false; // La tâche est à nouveau "en cours"
    this.completedTasks--;
    this.ongoingTasks++;
  } else {
    this.tasks[index].completed = true;
    this.completedTasks++;
    this.ongoingTasks--;
  }
},

editTask(index) {
  this.tasks[index].editing = true; // indique que la tâche est en cours d'édition
  this.name = this.tasks[index].name; // met à jour le nom dans le champ de saisie
  this.hours = this.tasks[index].hours; // met à jour le nombre d'heures dans le champ de saisie
  this.manager = this.tasks[index].manager; // met à jour le responsable dans le champ de saisie
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
  border: 2px solid rgb(38, 219, 62);
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
