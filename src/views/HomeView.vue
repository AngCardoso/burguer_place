<template>
  <div class="main-container">
    <Banner/>
    <BurguerForm/>
  </div>
</template>

<script>
import Banner from '@/components/banner.vue';
import BurguerForm from '@/components/burguerForm.vue';

export default{
  name: "Home",
  components: {
    Banner, BurguerForm
  }
}

</script>
