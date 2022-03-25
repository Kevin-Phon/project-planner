<template>
  <div class="home">
    <h1>Home</h1>
    <div v-for="project in projects" :key="project.id">
      <SingleProject :project='project' @delete="deleteProject" @complete="completeProject"></SingleProject>
    </div>
  </div>
</template>

<script>


import SingleProject from '../components/SingleProject'
export default {
  name: 'HomeView',
  data(){
    return{
      projects:[]
    }
  },
  components: {
    SingleProject,
  },
  methods:{
              //just name
    deleteProject(id){                    //change in local pj
      this.projects = this.projects.filter(project=>{
        return project.id != id
      })
    },
    completeProject(id){                  //change in local pj
      let findProject = this.projects.find(project=>{ //'find' will return an object
        return project.id === id
      }) //{...} now we have an object from local project(projects)
      findProject.complete =! findProject.complete
    }
  },
  mounted(){
    fetch('http://localhost:3000/projects')
    .then((response)=>{
      return response.json()
    })
    .then((datas)=>{
      this.projects = datas
    })
    .catch((err)=>{
      console.log(err)
    })
  }
}
</script>
