<template>
  <div id="app">
    <formSubmission
      
      :clientGroup="clientGroup"
      :doctorGroup="doctorGroup"
      
     />
  </div>
</template>

<script>
import formSubmission from '@/components/formSubmission.vue'
export default {
  name: 'App',
  data() {
    return {
      clientGroup: [
        {id: 1, title: 'VIP', completed: false},
        {id: 2, title: 'Проблемные', completed: false},
        {id: 3, title: 'ОМС', completed: false}
      ],
      doctorGroup: [
        {id: 1, title: 'Иванов', completed: false},
        {id: 2, title: 'Захаров', completed: false},
        {id: 3, title: 'Чернышева', completed: false}
      ]
    }
  },

  components: {
    formSubmission
  }
}
</script>