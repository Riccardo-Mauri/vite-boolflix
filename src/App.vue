<script>
//1- Importazione del componenente necessario che voglio aggiungere qua
import AppHeader from './components/AppHeader.vue';

export default {
  data() {
    return {
      count: 0
    }
  },
  //2- Dichiarazione del componente 
  components: {
    AppHeader
  },
  methods: {
    incrementCount(){
      this.count++;
    }
  }
}
</script>

<template>
  <div>
    <!--3- Utilizzo del componenete -->
    <AppHeader />
   <button @click="incrementCount()">
    {{ count }}
   </button>
  </div>
</template>

<style scoped>

</style>
