<script setup lang="ts">
import { onMounted, onUpdated, ref, watch } from 'vue';
interface Word {
    text: string,
    level: string,
    linkPronUS: string,
    linkPronUK: string,
}

const props = defineProps({
    words: {
        type: Array<Word>,
        required: true,
    },
});

watch(() => props.words, (newValue, oldValue: Word[]) => {
    index.value = 0;
    wordFront.value = props.words[index.value];
    wordBack.value = props.words[index.value];
});

let showFront = ref(true);
let wordFront = ref<Word>({text: "", level: "", linkPronUS: "", linkPronUK: ""});
let wordBack = ref<Word>({text: "", level: "", linkPronUS: "", linkPronUK: ""});
let index = ref<number>(0);

const changeSide = () => {
    showFront.value = !showFront.value;
    if (showFront.value) {
        wordFront.value = props.words[index.value];
    } else {
        wordBack.value = props.words[index.value];
    }
};

const playAudio = (link: string) => {
    link = 'https://www.oxfordlearnersdictionaries.com' + link;
    let audio = new Audio(link);
    audio.play();
}

document.body.onkeyup = function (e: KeyboardEvent) {
    //key up event
    if (e.key == "ArrowUp") {
        index.value++;
        changeSide();
        return;
    }

    //key down event
    if (e.key == "ArrowDown") {
        if (index.value === 0) return;
        index.value--;
        changeSide();
        return;
    }

    //key left event
    if (e.key == "ArrowLeft" && showFront.value) {
        playAudio(wordFront.value.linkPronUS);
        return;
    }
    if(e.key == "ArrowLeft" && !showFront.value) {
        playAudio(wordBack.value.linkPronUS);
        return;
    }

    //key right event
    if (e.key == "ArrowRight" && showFront.value) {
        playAudio(wordFront.value.linkPronUK);
        return;
    }
    if(e.key == "ArrowRight" && !showFront.value) {
        playAudio(wordBack.value.linkPronUK);
        return;
    }
}

onMounted(() => {
    index.value = 0;
    wordFront.value = props.words[index.value];
});


</script>

<template>
    <div class="word-flash-card w-screen h-screen bg-transparent overflow-hidden">
        <div class="word-flash-card-inner relative w-full h-full text-center transition-transform ease-in-out duration-500"
            :class="showFront ? 'show-front' : ' show-back'"
        >
            <div
                class="front absolute w-full h-full bg-beach-2 text-white flex justify-center items-center flex-col gap-4"
            >
                <div class="level text-3xl" :title="'Level ' + wordFront.level">{{wordFront.level}}</div>
                <div class="word font-bold text-8xl italic"> {{wordFront.text}}</div>
            </div>
            <div 
                class="back absolute w-full h-full bg-beach-1 text-white flex justify-center items-center flex-col gap-4"
            >
                <div class="level text-3xl" :title="'Level ' + wordBack.level">{{wordBack.level}}</div>
                <div class="word font-bold text-8xl italic"> {{wordBack.text}}</div>

            </div>
        </div>
    </div>
</template>

<style lang="less" scoped>
.word-flash-card {
    perspective: 9999px;

    &-inner.show-back {
        transform: rotateX(180deg);
    }

    &-inner {
        transform-style: preserve-3d;

        .front,
        .back {
            -webkit-backface-visibility: hidden;
            backface-visibility: hidden;
        }

        .back {
            transform: rotateX(180deg);
        }
    }
}
</style>