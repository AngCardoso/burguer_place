<template>
   <div id="main-banner">
    <h1>Make Your burguer</h1>
   </div>
</template>
<script>
export default{
    name: 'Banner',
}
</script>
<style scoped lang="scss">
#main-banner{
    
    background-image: url('/Projs/3_make_your_burguer/burguer_place/public/img/burger.jpg');
    background-position: 0 -250px; 
    background-size: cover;
    height: 500px;
    display: flex;
    align-items: center;
    justify-content: flex-start;  

        h1{
            color: #FFF;
            text-align: center;
            font-size: 60px;
            background-color: #222;
            padding: 20px 40px;
        }
}

</style>
