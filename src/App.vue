<script setup lang="ts">
import HelloWorld from "./components/HelloWorld.vue";
</script>

<template>
	<img alt="Vue logo" src="./assets/logo.png" />
	<HelloWorld />
</template>

<style lang="scss">
@use "styles/colors" as *;
@import "styles/setup";

#app {
	text-align: center;
	margin-top: 60px;
}
</style>
