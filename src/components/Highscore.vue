<template>
    <div class="flex space-x-4">
        <div v-if="highscores_geooest.length > 0" id="highscore" class="mt-10 p-5 rounded-xl bg-gray-800 bg-opacity-50">
            <h1 class="text-center text-3xl text-red-400 font-bold mb-5">Top 10</h1>
            <table class="m-auto">
                <tr class=" text-center font-bold border-b border-yellow-100">
                    <th class="px-3">Placering</th>
                    <th class="px-3">Navn</th>
                    <th class="px-3">Km forkert</th>
                </tr>
                <tr class="text-center" v-for="(highscore, idx) in topTen_geooest" :key="highscore.id">
                    <td>{{ idx + 1 }}.</td>
                    <td>{{ highscore.name }}</td>
                    <td>{{ Math.round(highscore.score * 10) / 10 }}</td>
                </tr>
            </table>
        </div>
    </div>
</template>

<script lang="ts">
import { computed, defineComponent, onMounted, ref } from 'vue'
import { Highscore } from "@/highscoreInterface";
import { supabase } from "@/supabase";

export default defineComponent({
    setup() {
        const highscores = ref<Highscore[]>([]);
        const topTen = computed(() => {
            return highscores.value.slice(0, 10)
        })
        
        onMounted( async () => {
            const fastfoodResponse = await supabase
                .from<Highscore>('highscore_geooest')
                .select('*')
                .order('score')
            highscores.value = fastfoodResponse.data
        })

        return {
            highscores_geooest: highscores,
            topTen_geooest: topTen,
        }
    },
})
</script>
