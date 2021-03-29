<template>
  <div class="home">
    <FilterNav @filterChange='filterChange' :currentFilter='currentFilter' />
    <div v-if="projects.length">
      <div v-for="project in filteredProjects" :key="project.id">
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
import FilterNav from '../components/FilterNav.vue'

export default {
  name: 'Home',
  components: { SingleProject, FilterNav },
  data() {
    return {
      projects: [],
      currentFilter: 'all'
    }
  },
  computed: {
    filteredProjects() {
      if(this.currentFilter === 'completed') {
        return this.projects.filter(project => project.complete)
      } 
      if(this.currentFilter === 'ongoing') {
        return this.projects.filter(project => !project.complete)
      } 
      return this.projects
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
    },
    filterChange(by) {
      this.currentFilter = by
    }
  }
}
</script>