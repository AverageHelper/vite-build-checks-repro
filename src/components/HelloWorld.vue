<script setup lang="ts">
import { computed } from "vue";

// FIXME: ts(2769) error: missing property `note`:
// vue-tsc misses this:
const thing = computed<Thing>(() => ({
	id: "1",
	name: "Bob",
	// note: "This is Bob's thing",
}));
</script>

<template>
	<h1>Thing {{ thing.id }}:</h1>
	<p>This thing belongs to {{ thing.name }}.</p>
	<p>This thing has a note: {{ thing.note || "TYPESCRIPT SAYS THERE SHOULD BE A NOTE HERE" }}</p>

	<p>
		Recommended IDE setup:
		<a href="https://code.visualstudio.com/" target="_blank">VSCode</a>
		+
		<a href="https://github.com/johnsoncodehk/volar" target="_blank">Volar</a>
	</p>

	<p>See <code>README.md</code> for more information.</p>
</template>

<style scoped lang="scss">
@use "styles/colors" as *;

a {
	color: color($green);
}

label {
	margin: 0 0.5em;
	font-weight: bold;
}
</style>
