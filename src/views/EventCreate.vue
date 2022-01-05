<template>
  <h1> Create an event </h1>

  <div class="form-container">

    <form @submit.prevent="onSubmit">
      <label>Select a category:</label>
      <select v-model="event.category">
        <option v-for="option in categories" :value="option" :key="option" :selected="option === event.category">
          {{ option }} </option>
      </select>

      <h3>Name & describe your event</h3>

      <BaseInput
        v-model="event.title"
        label="Title"
      />

      <BaseInput
        v-model="event.description"
        label="Description" 
      />

      <h3>Where is your event?</h3>

      <BaseInput
        v-model="event.location" 
        label="Location" 
      />

      <h3>When is your event?</h3>

      <BaseInput
        v-model="event.date"
        label="Date"
      />

      <BaseInput
        v-model="event.time" 
        label="Time" 
      />

      <button type="submit">Submit</button>

    </form>

  </div>

</template>


<script>
  import {v4 as uuidv4} from 'uuid'
  import BaseInput from "@/components/BaseInput.vue";

  export default {
    components: {
      BaseInput
    },
    data() {
      return {
        categories: [
          'sustainability',
          'nature',
          'animal welfare',
          'housing',
          'education',
          'food',
          'community'
        ],
        event: {
          id: '',
          category: '',
          title: '',
          description: '',
          location: '',
          date: '',
          time: '',
          organizer: ''
        }
      }
    },
    methods: {
      onSubmit() {
        const event = {
          ...this.event,
          id: uuidv4(),
          organizer: this.$store.state.user
        }
        this.$store.dispatch('createEvent', event)
          .then(() => {
            this.$router.push({
              name: 'EventDetails',
              params: {
                id: event.id
              }
            })
          })
          .catch(error => {
            this.$router.push({
              name: 'ErrorDisplay',
              params: {
                error: error
              }
            })
          })
      }
    },

  }
</script>


<style>

</style>