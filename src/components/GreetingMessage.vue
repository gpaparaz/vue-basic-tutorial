<script>
import { computed, inject } from 'vue'

export default {
  props: ['firstName', 'lastName'],

  setup(props) {

    const fullName = computed(() => {
      return props.firstName + ' ' + props.lastName
    })

    // receive 'username'
    const uName = inject('username')


    return {
      fullName, uName
    }
  }
}
</script>

<template>
  <p>Hello {{ fullName }}</p>
  <p>Username from parent: {{ uName }}</p>
</template>