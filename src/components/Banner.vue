<template>
<div id="banner">
    <h1>Dev Burger</h1>   
</div>
</template>

<script>
export default {
    name: "Banner",
}
</script>

<style scoped>
#banner {
    background-image: url('/img/burger.jpg');
    background-position: 0 -250px;
    background-size: cover;
    height: 500px;
    display: flex;
    align-items: center;
    justify-content: flex-start;
}
#banner h1 {
    color: #fff;
    text-align: center;
    font-size: 3em;
    background-color: #222;
    padding:20px 40px;
    
}
</style>