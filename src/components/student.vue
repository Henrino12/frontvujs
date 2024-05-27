<template>
    <div class ="container">
      <h2>Ajout Etudiant</h2>
                    <form @submit.prevent="save">
                    
                    <div class="form-group">
                        <label>Student name</label>
                        <input type="text" v-model="student.name" class="form-control"  placeholder="Prenom">
                    
                    </div>
                    <div class="form-group">
                        <label>Student last_name</label>
                        <input type="text" v-model="student.last_name" class="form-control"  placeholder="Nom">
                    
                    </div>

                    <div class="form-group">
                        <label>Student Address</label>
                        <input type="email" v-model="student.address" class="form-control"  placeholder="Student Address">
                    
                    </div>

                    
                    <div class="form-group">
                        <label>Phone</label>
                        <input type="text" v-model="student.phone" class="form-control"  placeholder="Phone">
                    
                    </div>
                    
                    <button type="submit" class="btn btn-primary">Save</button>
                    </form> 
 
 
<h2>Liste des étudiants</h2>
      <table class="table table-hover">
  <thead>
    <tr>
      <th scope="col">ID</th>
      <th scope="col">Prenom</th>
      <th scope="col">Nom</th>
      <th scope="col">Address</th>
      <th scope="col">Phone</th>
      <th scope="col">Action</th>
    </tr>
  </thead>

  <tbody>
    <tr v-for="student in result" v-bind:key="student.id">
          
          <td>{{ student.id }}</td>
          <td>{{ student.name }}</td>
          <td>{{ student.last_name }}</td>
          <td>{{ student.address }}</td>
          <td>{{ student.phone }}</td>
          <td>
            <button type="button" class="btn btn-warning" @click="edit(student)">Edit</button>
            <button type="button" class="btn btn-danger"  @click="remove(student)">Delete</button>
          </td>
        </tr>
    
  </tbody>

  
</table>
     
    </div>

  </template>

  <script>
    
   import axios from 'redaxios';

 
  export default {
    // eslint-disable-next-line vue/multi-word-component-names
    name: 'Student',
    data () {
      return {
        result: {},
        student:{
                   id: '',
                   name: '',
                   last_name: '',
                   address: '',
                   phone: ''
 
 
        }
      }
    },
    created() { 
        this.StudentLoad();
    },
    mounted() {
          console.log("mounted() called.......");
         
      },
 
    methods: {
           StudentLoad()
           {
                 var page = "http://127.0.0.1:8000/api/student";
                 axios.get(page)
                  .then(
                      ({data})=>{
                        console.log(data);
                        this.result = data.students;
                        console.log(this.result);
                      }
                  );
           },
           save()
           {
            if(this.student.id == '')
              {
                this.saveData();
              }
              else
              {
                this.updateData();
              }       
 
           },
           saveData()
           {
            axios.post("http://127.0.0.1:8000/api/student", this.student)
            .then(
              ({data})=>{
                alert("voulez vous enregistrer cette étudiant ?");
                this.StudentLoad();
                 this.student.name = '';
                 this.student.last_name = '';
                  this.student.address = '',
                  this.student.phone = ''
                   this.id = ''
              }
            )
 
           },
           edit(student)
           {
            this.student = student;
           
           },
           updateData()
           {
              var editrecords = 'http://127.0.0.1:8000/api/student/'+ this.student.id;
              axios.put(editrecords, this.student)
              .then(
                ({data})=>{
                  this.student.name = '',
                  this.student.last_name = '',
                  this.student.address = '',
                  this.student.phone = ''
                  this.id = ''
                  alert("Updated!!!");
                  this.StudentLoad();
                }
              );
 
           },
 
           remove(student){
 
             var url = `http://127.0.0.1:8000/api/student/${student.id}`;
 
 
 
             // var url = 'http://127.0.0.1:8000/api/student/'+ student.id;
              axios.delete(url);
              alert("Etudiant supprimé avec succès !");
              this.StudentLoad();
            }
      }
  }
  </script>