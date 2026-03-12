<template>

<h1>The Window Object</h1>
<h2>The localStorage Property</h2>

<p>Saved name is:</p>
<p id="demo"></p>

</template>

<script setup>
// Set Item
localStorage.setItem("lastname", "Smith");
// Retrieve
document.getElementById("demo").innerHTML = localStorage.getItem("lastname");
</script>

<style scoped>

</style>