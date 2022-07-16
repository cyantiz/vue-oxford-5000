<script setup lang="ts">
// const props = defineProps({
// });
</script>

<template>
    <div id="instruction" class="bg-black bg-opacity-30 fixed left-0 top-0 z-[100]  w-screen h-screen flex justify-center items-center">
        <div class="instruction-dialog w-screen h-screen sm:w-auto sm:h-auto p-10 bg-white flex flex-col gap-5 items-center">
            <div class="instruction-text flex flex-col gap-2">
                <div class="flex gap-2">
                    <div class="btn-up bg-black w-6 h-6"></div> 
                    <span> : Previous word</span>
                </div>
                <div class="flex gap-2">
                    <div class="btn-down bg-black w-6 h-6"></div> 
                    <span> : Next word</span>
                </div>
                <div class="flex gap-2">
                    <div class="btn-left bg-black w-6 h-6"></div> 
                    <span> : UK's accent pronunciation</span>
                </div>
                <div class="flex gap-2">
                    <div class="btn-right bg-black w-6 h-6"></div> 
                    <span> : US's accent pronunciation</span>
                </div>
            </div>
            <div class="touch-screen-instruction-text w-[400px] px-3 text-sm text-red-500 text-center">
                For touch screen, i.e. mobile, tablet... press on the corresponding position on screen
                <br>
                e.g., press bottom side for next word
            </div>

            <div class="source flex flex-col sm:flex-row items-center gap-1">
                Words source <a target="_blank" class="bg-sky-200 text-black flex gap-2 items-center" href="https://www.oxfordlearnersdictionaries.com/wordlists/oxford3000-5000"><div class="arrow-right bg-black w-5 h-5" /> Oxford Learner's Dictionaries</a>
            </div>
            
            <div class="got-it bg-black text-white p-3 cursor-pointer" @click="$emit('close')">GOT IT!</div>
        </div>
    </div>
    
</template>

<style lang="less" scoped>
.btn-up {
    -webkit-mask: url(../assets/arrow-up-s-fill.svg) no-repeat center;
	mask: url(../assets/arrow-up-s-fill.svg) no-repeat center;
	-webkit-mask-size: 100%;
	mask-size: 100%;
}
.btn-down {
    -webkit-mask: url(../assets/arrow-down-s-fill.svg) no-repeat center;
	mask: url(../assets/arrow-down-s-fill.svg) no-repeat center;
	-webkit-mask-size: 100%;
	mask-size: 100%;
}
.btn-left {
    -webkit-mask: url(../assets/arrow-left-s-fill.svg) no-repeat center;
	mask: url(../assets/arrow-left-s-fill.svg) no-repeat center;
	-webkit-mask-size: 100%;
	mask-size: 100%;
}
.btn-right {
    -webkit-mask: url(../assets/arrow-right-s-fill.svg) no-repeat center;
	mask: url(../assets/arrow-right-s-fill.svg) no-repeat center;
	-webkit-mask-size: 100%;
	mask-size: 100%;
}
.arrow-right {
    -webkit-mask: url(../assets/arrow-right.svg) no-repeat center;
	mask: url(../assets/arrow-right.svg) no-repeat center;
	-webkit-mask-size: 100%;
	mask-size: 100%;
}
</style>