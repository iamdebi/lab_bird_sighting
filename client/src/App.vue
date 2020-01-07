<template>
  <div id="app">
    <sightings-form />
    <sightings-grid :sightings="sightings" />
  </div>
</template>

<script>
import SightingsForm from './components/SightingsForm';
import SightingsGrid from './components/SightingsGrid';
import SightingService from './services/SightingService.js';
import { eventBus } from './main';

export default {
  name: 'app',
  data () {
    return {
      sightings: []
    }
  },
  components: {
    'sightings-form': SightingsForm,
    'sightings-grid': SightingsGrid
  },
  mounted(){
    this.fetchData(),
		eventBus.$on('bird-added', (bird) => {
			this.sightings.push(bird)
		}),
    eventBus.$on('bird-deleted', (id) => {
      let index = this.sightings.findIndex(bird => bird._id === id)
      this.sightings.splice(index, 1)
    })
  },
  methods: {
    fetchData(){
      SightingService.getSightings()
      .then(sightings => this.sightings = sightings);
    }
  }
}
</script>

<style>
html {
  height: 100%;
}

body {
  background: url('./assets/birds-background.jpg') no-repeat;
  height: 100%;
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;

}
</style>
