<script setup lang="ts">
import { ref } from 'vue';
import WordFlashCard from '../components/WordFlashCard.vue';
import Instruction from '../components/Instruction.vue';
import Settings from '../components/Settings.vue';
import { computed } from '@vue/reactivity';
interface Word {
    text: string,
    level: string,
    linkPronUS: string,
    linkPronUK: string,
}

let filter = ref<string>("");
let showSettings = ref(false);
let showInstruction = ref(false);
let words = ref<Word[]>([]);
let intervalTime = ref<number>(0);
let isAutoPronounce = ref<boolean>(false);
let autoPronounceAccent = ref<string>("US");
const wordFlashCardRef = ref();

const shuffle = (array: Array<Word>) => {
    for (let i = array.length - 1; i > 0; i--) {
        let j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]];
    }
}

const saveSettings = (e: any) => {
	intervalTime.value = 1000;
	filter.value = e.filter;
	intervalTime.value = e.intervalTime;
	showSettings.value = false;
	isAutoPronounce.value = e.isAutoPronounce;
	autoPronounceAccent.value = e.autoPronounceAccent;
}

let filteredWords = computed(() => {
	if (filter.value) {
		return words.value.filter(item => item.level === filter.value);
	}
	return words.value;
})


const fetchData = async () => {
	try {
		const wordsData = await fetch('https://cyantiz.github.io/words.json').then(r => r.json());
		const levelsData = await fetch('https://cyantiz.github.io/levels.json').then(r => r.json());
		const linkPronUSData = await fetch('https://cyantiz.github.io/pron_us_mp3.json').then(r => r.json());
		const linkPronUKData = await fetch('https://cyantiz.github.io/pron_uk_mp3.json').then(r => r.json());

		words.value = wordsData.map((item: number, index: number) => {
			return {
				text: item,
				level: levelsData[index],
				linkPronUS: linkPronUSData[index],
				linkPronUK: linkPronUKData[index],
			}
		});
		// shuffle words twice
		shuffle(words.value)
		shuffle(words.value)
	}

	catch (err) {
		alert('cant fetch data');
	}
}

await fetchData();


</script>

<template>	
	<div class="bg-beach-1 h-screen relative">
		<div v-show="intervalTime === 0" class="function-on-screen-touch flex flex-col justify-between fixed left-0 top-50 z-10 w-full h-screen">
            <div class="top h-1/2" @click="wordFlashCardRef.toPrevWord" />
            <div class="bottom h-1/2" @click="wordFlashCardRef.toNextWord" />
        </div>
		<div v-show="intervalTime === 0" class="function-on-screen-touch flex justify-between fixed left-0 top-[50%] -translate-y-[50%] z-20 w-full h-48">
            <div class="left w-2/5" @click="wordFlashCardRef.playAudio('UK')" />
            <div class="right w-2/5" @click="wordFlashCardRef.playAudio('US')" />
        </div>
		<div
			class="instruction-btn bg-gray-400 hover:bg-white transition-all fixed left-4 top-4 z-40 w-6 h-6 cursor-pointer"
			@click="showInstruction = true"
		>
		</div>
		<div
			class="settings-btn bg-gray-400 hover:bg-white transition-all fixed z-40 top-4 left-14 w-6 h-6 cursor-pointer"
			@click="showSettings = true"
		>
		</div>
		<Instruction v-show="showInstruction" @close="showInstruction = false" />
		<Settings
			v-if="showSettings"
			:current-filter="filter" 
			:interval-time="intervalTime" 
			@close="showSettings = false"
			@save="(e) => saveSettings(e)"
			:is-auto-pronounce="isAutoPronounce" 
			:auto-pronounce-accent="autoPronounceAccent" 
		/>
		<WordFlashCard
			:words="filteredWords"
			:interval-time="intervalTime" 
			:is-auto-pronounce="isAutoPronounce" 
			:auto-pronounce-accent="autoPronounceAccent" 
			ref="wordFlashCardRef"
		/>
	</div>
</template>

<style lang="less" scoped>
.instruction-btn {
	-webkit-mask: url(../assets/information-line.svg) no-repeat center;
	mask: url(../assets/information-line.svg) no-repeat center;
	-webkit-mask-size: 100%;
	mask-size: 100%;
}
.settings-btn {
	-webkit-mask: url(../assets/settings-line.svg) no-repeat center;
	mask: url(../assets/settings-line.svg) no-repeat center;
	-webkit-mask-size: 100%;
	mask-size: 100%;
}

.function-on-screen-touch {
	display: none
}
@media only screen and (hover: none) and (pointer: coarse){
	.function-on-screen-touch {
		display: flex;
	}

}


</style>