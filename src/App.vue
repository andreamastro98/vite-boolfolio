<script >

import AppHeader from '../src/components/AppHeader.vue';


export default {
  components:{
    AppHeader
  },
  data(){
    return{
      
    }
  },
  mounted(){
    
  },
  methods:{
    
  },
}

</script>

<template>
  <AppHeader></AppHeader>
  <router-view></router-view>
</template>

<style lang="scss">
@use './style/main.scss';

</style>
