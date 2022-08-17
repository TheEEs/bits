<template>
    <div class="h-screen w-full flex items-center justify-center font-mono text-xl">
        <div>
            <section id="bitsA" class="text-3xl cursor-pointer">
                <span @click="bitsA[k] = Math.abs(i - 1)" v-for="i, k in bitsA" :key="`bitA${k}`">{{ bit(i)
                }}</span>
            </section>
            <strong class="text-sm font-normal">({{ decA }})</strong>
            <div>
                <select class="py-4" v-model="operator" name="operator" id="">
                    <option value="and">AND</option>
                    <option value="or">OR</option>
                    <option value="xor">XOR</option>
                </select>
            </div>
            <section id="bitsB" class="text-3xl cursor-pointer">
                <span @click="bitsB[k] = Math.abs(i - 1)" v-for="i, k in bitsB" :key="`bitB${k}`">{{ bit(i) }}</span>
            </section>
            <strong class="text-sm font-normal">({{ decB }})</strong>

            <div id="line" class="border-2 border-slate-600 mt-6 mb-4"></div>
            <section id="bitC" class="text-3xl cursor-default">
                <span v-for="i, k in result" :key="`bitC${k}`">{{ bit(i) }}</span>
            </section>
            <strong class="text-sm font-normal">({{ decC }})</strong>
            <button @click="moveToA" class="float-right px-2 py-3
             bg-sky-500 mt-4 font-normal text-sm text-slate-100 border-b-2 border-b-sky-700 active:border-b-0">Copy to
                A</button>
        </div>
    </div>
</template>

<script setup>
import { computed, ref } from "vue";
const bitsA = ref([0, 0, 0, 0, 0, 0, 0, 0])

const bitsB = ref([0, 0, 0, 0, 0, 0, 0, 0])

const operator = ref("and");

const result = computed(() => {
    switch (operator.value) {
        case "and": return [...Array(8).keys()].map((i) => bitsA.value[i] & bitsB.value[i])
        case "or": return [...Array(8).keys()].map((i) => bitsA.value[i] | bitsB.value[i])
        case "xor": return [...Array(8).keys()].map((i) => bitsA.value[i] ^ bitsB.value[i])
    }
})

const decA = computed(() => {
    return parseInt(bitsA.value.join(""), 2)
})

const decB = computed(() => {
    return parseInt(bitsB.value.join(""), 2)
})


const decC = computed(() => {
    return parseInt(result.value.join(""), 2)
})

function moveToA() {
    bitsA.value = result.value
}

function bit(b) {
    if (b == "0")
        return "🌑";
    else if (b == "1")
        return "🌕";
}
</script>