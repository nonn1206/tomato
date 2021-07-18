<template lang="pug">
#app
  #top
    b-container
      h2 トマトの時計
      hr
      b-btn.btn1(to='/list') Check list
      b-btn.btn2(to='/settings') Daily report
    Home
  #contect
    //- b-container(cols="12")
    //- Home
    keep-alive
      router-view
        List
</template>

<script>
import Home from './views/Home.vue'
import List from './views/List.vue'

export default {
  name: 'App',
  components: {
    Home,
    List
  }
}
</script>
