<script setup lang="ts">
import { ref } from 'vue';

const props = defineProps({
    currentFilter: String,
    intervalTime: Number,
    isAutoPronounce: {
        type: Boolean,
        required: true,
    },
    autoPronounceAccent: {
        type: String,
        required: true,
    }
});

let filter = ref(props.currentFilter)
let intervalTime = ref(props.intervalTime)
let isAutoPronounce = ref(props.isAutoPronounce)
let autoPronounceAccent = ref(props.autoPronounceAccent)
const LEVELS = [
    'A1',
    'A2',
    'B1',
    'B2',
    'C1',
];

const handleDecreaseTime = () => {
    if (intervalTime.value === undefined) return;
    if (intervalTime.value === 1000) return;
    intervalTime.value = intervalTime.value - 500;
}
const handleIncreaseTime = () => {
    if (intervalTime.value === undefined) return;
    if (intervalTime.value === 10000) return;
    intervalTime.value = intervalTime.value + 500;
}

</script>

<template>
    <div  id="settings" class="bg-black bg-opacity-30 fixed left-0 top-0 z-[100]  w-screen h-screen flex justify-center items-center">
        <div class="settings-dialog w-screen h-screen sm:w-auto sm:h-auto p-10 bg-white flex flex-col gap-5 items-center">
            <div class="settings-options flex flex-col gap-5">

                <div class="filter-level">
                    <div class="title mb-2">Filter</div>
                    <ul class="filters grid grid-cols-3 gap-x-5 gap-y-3">
                        <li
                            :class="!filter ? 'selected' : ''"
                            class="filter-item border border-black w-[80px] text-center py-1 cursor-pointer"
                            @click="filter = ''"
                        >
                            All
                        </li>
                        <li
                            v-for="level in LEVELS"
                            :key="level"
                            :class="filter === level ? 'selected' : ''" 
                            class="filter-item border border-black w-[80px] text-center py-1 cursor-pointer"
                            @click="filter = level"
                        >
                            {{level}}
                        </li>
                    </ul>
                </div>

                <div class="interval flex flex-col">
                    <div class="title mb-2">Auto change word after</div>
                    <div class="flex items-center gap-2">
                        <div
                            :class="intervalTime === 0 ? 'selected' : '' " class="border border-black w-[150px] text-center py-1 cursor-pointer"
                            @click="intervalTime = 0"
                        >
                        OFF
                        </div>
                        <div
                            v-if="intervalTime === 0"
                            class="border border-black w-[150px] text-center py-1 cursor-pointer"
                            @click="intervalTime = 1000"
                        >
                            Constant time
                        </div>     
                        <div class="time relative flex items-center h-[34px] border border-white bg-black text-white" v-if="intervalTime !== 0">
                            <input 
                                type="text" :value="intervalTime" 
                                class="border-none border-0 w-[124px] px-3 outline-none bg-black text-white"
                                readonly
                            >
                            <div class="w-[25px] text-center absolute right-[60px] select-none">
                                ms
                            </div>
                            <div class="btns h-full w-auto">
                                <div class="btn-up-container hover:bg-white h-1/2 cursor-pointer" @click="handleIncreaseTime">
                                    <div class="btn-up hover:bg-black bg-white h-full w-6"></div>
                                </div>
                                <div class="btn-down-container hover:bg-white h-1/2 cursor-pointer" @click="handleDecreaseTime">
                                    <div class="btn-down hover:bg-black bg-white h-full w-6"></div>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="note text-sm text-red-700 w-[308px]">Note: arrow keys function will be disabled during auto-changing</div>
                </div>

                <div class="auto-pronounce">
                <div class="title mb-2">Auto pronounce on word changing</div>
                <div class="options flex gap-2">
                    <div
                        :class="!isAutoPronounce ? 'selected' : '' " class="border border-black w-1/3 text-center py-1 cursor-pointer"
                        @click="isAutoPronounce = false"
                    >
                        OFF
                    </div>
                    <div
                        :class="isAutoPronounce && autoPronounceAccent === 'US' ? 'selected' : '' " class="border border-black w-1/3 text-center py-1 cursor-pointer"
                        @click="isAutoPronounce = true; autoPronounceAccent = 'US'"
                    >
                        US
                    </div>
                    <div
                        :class="isAutoPronounce && autoPronounceAccent === 'UK' ? 'selected' : '' " class="border border-black w-1/3 text-center py-1 cursor-pointer"
                        @click="isAutoPronounce = true; autoPronounceAccent = 'UK'"
                    >
                        UK
                    </div>
                    

                </div>
            </div>
            
            
            </div>

            
            
            <div class="flex flex-col items-stretch gap-2 w-[308px]">
                <div class="save text-center bg-black text-white p-2 cursor-pointer" @click="$emit('save', {filter, intervalTime, isAutoPronounce, autoPronounceAccent})">SAVE</div>
                <div class="close text-center bg-white border border-black  text-black p-2 cursor-pointer" @click="$emit('close')">CLOSE</div>
            </div>
        </div>
    </div>
    
</template>

<style lang="less" scoped>

.selected {
    background: black;
    color: white;
}
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
</style>