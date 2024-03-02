<script setup>
    import { Header, Footer } from '.';
</script>
<template>
   <div class="container">
    <Header />
        <slot /> 
        
    <Footer/>
   </div>
</template>