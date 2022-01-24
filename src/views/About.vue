<template>
  <h1>This is an about page</h1>
</template>

<script>
import {reactive} from 'vue';

export default {

  setup(props, context) {
    let data = reactive({
      todos: [
        {id: []}
      ]
    });
    return {};
  }
}

</script>
