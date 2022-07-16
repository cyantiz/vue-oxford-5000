<script setup lang="ts">
import { stringify } from 'querystring';
import { ref } from 'vue';

const props = defineProps({
    currentFilter: String,
});

let filter = ref(props.currentFilter)

const LEVELS = [
    'A1',
    'A2',
    'B1',
    'B2',
    'C1',
];

</script>

<template>
    <div  id="settings" class="bg-black bg-opacity-30 fixed left-0 top-0 z-20  w-screen h-screen flex justify-center items-center">
        <div class="settings-dialog p-10 bg-white flex flex-col gap-5 items-center">
            <div class="settings-options">
                <div class="mb-2">Filter</div>
                <ul class="filters grid grid-cols-2 gap-2">
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
            
            <div class="flex flex-col items-stretch gap-2 w-full">
                <div class="save text-center bg-black text-white p-2 cursor-pointer" @click="$emit('save', filter)">SAVE</div>
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
</style>