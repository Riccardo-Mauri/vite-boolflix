<script>
export default {
  data() {
    return {
      message: 'Template Vite + Vue'
    }
  },
  
}
</script>

<template>
  <div>
    <header>
      <h1>
        {{ message }}
      </h1>

    </header>
  </div>
</template>

<style scoped>
header{
  background-color: aquamarine;
  text-align: center;
  padding: 25px 0;
}
h1{
  color: lightcoral;
}
</style>
