<template>
  <div class="main-container">
    <h1>Gerenciar pedidos</h1>
    <Dashboard/>
  </div>
</template>
<script>
import Dashboard from '@/components/dashboard.vue';
export default{
  name: "pedidos",
  components:{
    Dashboard
  }
}
</script>