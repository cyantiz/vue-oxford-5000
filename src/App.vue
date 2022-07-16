<script setup lang="ts">
import { ref, onMounted, Suspense } from 'vue';
import WordFlashCard from './components/WordFlashCard.vue';
import Words from './pages/Words.vue';


</script>

<template>
	<Suspense>
		<Words />
	</Suspense>
</template>

<style lang="less">
:root {
	--beach-1: #094C93;
	--beach-2: #0C73B2;
	--beach-3: #1C96BA;
	--beach-4: #3CBCC7;
	--beach-5: #EAE1C8;
	--beach-6: #DFC39D;

}

body {
	font-family: 'JetBrains Mono', monospace;
}

.bg-beach-1 {
	background-color: var(--beach-1);
}

.bg-beach-2 {
	background-color: var(--beach-2);
}

.bg-beach-3 {
	background-color: var(--beach-3);
}

.bg-beach-4 {
	background-color: var(--beach-4);
}

.bg-beach-5 {
	background-color: var(--beach-5);
}

.bg-beach-6 {
	background-color: var(--beach-6);
}

.text-beach-1 {
	color: var(--beach-1);
}

.text-beach-2 {
	color: var(--beach-2);
}

.text-beach-3 {
	color: var(--beach-3);
}

.text-beach-4 {
	color: var(--beach-4);
}

.text-beach-5 {
	color: var(--beach-5);
}

.text-beach-6 {
	color: var(--beach-6);
}
</style>

