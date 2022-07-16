<script setup lang="ts">
import { onMounted, ref, watch } from 'vue';
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
    intervalTime: {
        type: Number,
        required: true,
    },
    isAutoPronounce: {
        type: Boolean,
        required: true,
    },
    autoPronounceAccent: {
        type: String,
        required: true,
    }
});

watch(() => props.words, (newValue, oldValue: Word[]) => {
    index.value = 0;
    wordFront.value = props.words[index.value];
    wordBack.value = props.words[index.value];
});
watch(() => props.intervalTime, (newValue, oldValue: number) => {
    clearInterval(interval);
    ("interval cleared")
    if (newValue === 0) return;
    interval = setInterval(() => {
        index.value = (index.value + 1) % props.words.length;
        changeSide();
    }, newValue);
    ("interval created")
});

let showFront = ref(true);
let wordFront = ref<Word>({text: "", level: "", linkPronUS: "", linkPronUK: ""});
let wordBack = ref<Word>({text: "", level: "", linkPronUS: "", linkPronUK: ""});
let index = ref<number>(0);
let interval: any;
let audio: HTMLAudioElement = new Audio();

const changeSide = () => {
    showFront.value = !showFront.value;
    if (showFront.value) {
        wordFront.value = props.words[index.value];
    } else {
        wordBack.value = props.words[index.value];
    }
    // auto pronounce
    if (!props.isAutoPronounce) return;
    if (props.autoPronounceAccent === "US") {
        playAudio("US");
        return;
    }
    playAudio("UK");
};

const toNextWord = () => {
    index.value++;
    changeSide();
}
const toPrevWord = () => {
    if (index.value === 0) return;
    index.value--;
    changeSide();   
}

const playAudio = (accent: string) => {
    let link = 'https://www.oxfordlearnersdictionaries.com';
    if (accent === "US") {
        link = link + (showFront.value ? wordFront.value.linkPronUS : wordBack.value.linkPronUS);
    }
    else {
        link = link + (showFront.value ? wordFront.value.linkPronUK : wordBack.value.linkPronUK);
    }
    // if (!audio.paused) {
    //     return;
    // }
    audio = new Audio(link);
    audio.play();
}

document.body.onkeyup = function (e: KeyboardEvent) {
    //disabled when auto word changing on
    if(props.intervalTime !== 0) return;

    //key up event
    if (e.key == "ArrowDown") {
        toNextWord();
        return;
    }

    //key down event
    if (e.key == "ArrowUp") {
        toPrevWord();
        return;
    }

    //key left event
    if(e.key == "ArrowLeft") {
        playAudio("US");
        return;
    }

    //key right event
    if(e.key == "ArrowRight") {
        playAudio("UK");
        return;
    }
}

onMounted(() => {
    index.value = 0;
    wordFront.value = props.words[index.value];
});



defineExpose({
    toNextWord,
    toPrevWord,
    playAudio,
})



</script>

<template>
    <div class="word-flash-card w-screen h-48 fixed z-[5] top-[50%] left-0 -translate-y-[50%] flex justify-center items-center bg-transparent overflow-hidden">
        <div 
            class="word-flash-card-inner relative flex justify-center items-center text-center transition-transform ease-in-out duration-500"
            :class="showFront ? 'show-front' : ' show-back'"
        >
            <div
                class="front absolute w-screen h-48 bg-beach-1 text-white flex justify-center items-center flex-col gap-4"
            >
                <div 
                    :class="intervalTime !== 0 ? 'select-none' : '' " 
                    class="level text-2xl sm:text-3xl" :title="'Level ' + wordFront.level"
                >
                    {{wordFront.level}}
                </div>
                <div 
                    :class="intervalTime !== 0 ? 'select-none' : '' " 
                    class="word font-bold text-4xl sm:text-8xl italic transition-all"
                >
                    {{wordFront.text}}
                </div>
            </div>
            <div 
                class="back absolute w-screen h-48 bg-beach-1 text-white flex justify-center items-center flex-col gap-4"
            >
                <div 
                    :class="intervalTime !== 0 ? 'select-none' : '' " 
                    class="level text-2xl sm:text-3xl" :title="'Level ' + wordBack.level"
                >
                    {{wordBack.level}}
                </div>
                <div
                    :class="intervalTime !== 0 ? 'select-none' : '' " 
                    class="word font-bold text-4xl sm:text-8xl italic transition-all"
                > 
                    {{wordBack.text}}
                </div>
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