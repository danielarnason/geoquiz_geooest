<template>
    <div class="start fixed w-full h-full bg-black bg-opacity-50 z-10 flex justify-center items-center flex-col text-yellow-100">
        <div class="intro text-center max-w-screen-sm">
            <h2 class="text-4xl mb-6">Velkommen til <span class="font-bold text-red-400">Rådhus Quizzen</span>
            </h2>
            <p>Hvor ligger landets rådhuse?</p>
        </div>
        <div class="help">
            <p>Klik på kortet og bekræft dit gæt</p>
        </div>

        <div id="initials" class="text-red-400 mt-4">
            <input placeholder="Skriv dit navn" class=" text-center shadow appearance-none border rounded w-full py-2 px-3 leading-tight focus:outline-none focus:shadow-outline" v-model="initials" @input="initialsChange" type="text">
        </div>
        

        <div id="category" class="text-3xl">
            <button :disabled="activateButton" @click="showStart('raadhuse')" :class="dynClass">Start</button>
            <!-- <button :disabled="activateButton" @click="showStart('fortidsminder')" :class="dynClass">Fortidsminder</button> -->
        </div>
        <Highscore />
    </div>
</template>

<script lang="ts">
import { computed, defineComponent, ref } from 'vue'
import Highscore from '@/components/Highscore.vue'

export default defineComponent({
    components: {
        Highscore
    },
    setup(props, { emit }) {

        const initials = ref<string>("");
        const activateButton = computed(() => {
            if (initials.value.length > 0) {
                return false
            } else {
                return true
            }
        })
        const dynClass = computed(() => {
            if (initials.value.length > 0) {
                return "bg-red-400 pb-1 m-3 mt-6 w-56 rounded hover:bg-red-900"
            } else {
                return "bg-gray-800 m-3 w-56 mt-6 rounded pb-1 disabled:opacity-50 cursor-not-allowed"
            }
        })
        
        const showStart = (category: string) => {
            emit('showStart', {state: false, category: category})
        }

        const initialsChange = () => {
            emit('initialsChange', initials.value)
        }

        return {
            showStart,
            initialsChange,
            initials,
            activateButton,
            dynClass
        }
    },
})
</script>
