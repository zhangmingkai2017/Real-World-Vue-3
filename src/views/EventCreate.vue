<template>
  <h1> Create an event </h1>

  <div class="form-container">

    <form @submit.prevent="onSubmit">

      <BaseSelect
        :options="categories"
        v-model="event.category"
        label="Select a category"
      />

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

      <h3>Extras</h3>
      <div>
        <BaseCheckbox
          v-model="event.extras.catering"
          label="Catering"
        />
      </div>

      <div>
        <BaseCheckbox
          v-model="event.extras.music"
          label="Live music"
        />
      </div>

      <h3>Are pets allowed</h3>
      <div>
        <BaseRadioGroup
          v-model="event.pets"
          name="pets"
          :options="petOptions"
        />
      </div>

      <button type="submit">Submit</button>

    </form>

  </div>

</template>


<script>
  import {v4 as uuidv4} from 'uuid'
  import BaseInput from "@/components/BaseInput.vue";
  import BaseSelect from "@/components/BaseSelect.vue";
  import BaseCheckbox from "@/components/BaseCheckbox.vue";
  import BaseRadioGroup from "@/components/BaseRadioGroup.vue";

  export default {
    components: {
      BaseInput,
      BaseSelect,
      BaseCheckbox,
      BaseRadioGroup
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
          organizer: '',
          pets: '1',
          extras: {
            catering: false,
            music: false
          }
        },
        petOptions: [
          { label: 'Yes', value: 1 },
          { label: 'No', value: 0 }
        ]
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