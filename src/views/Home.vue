<template>
  <div class="home">
    <div v-if="projects.length">
      <div v-for="project in projects" :key="project.id">
        <SingleProject 
          :project="project" 
          @deleteProject='deleteSingleProject' 
          @completeProject='completeSingleProject'
        />
      </div>
    </div>
  </div>
</template>

<script>
import SingleProject from '../components/SingleProject.vue'

export default {
  name: 'Home',
  components: { SingleProject },
  data() {
    return {
      projects: []
    }
  },
  mounted() {
    fetch('http://localhost:3000/projects')
      .then(res => res.json())
      .then(data => this.projects = data)
      .catch(err => console.log(err.message))
  },
  methods: {
    deleteSingleProject(id) {
      this.projects = this.projects.filter(project => project.id !== id)
    },
    completeSingleProject(id) {
      let p = this.projects.find(project => project.id === id)
      p.complete = !p.complete
    }
  }
}
</script>