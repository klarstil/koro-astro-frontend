<script setup lang="ts">
import 'vue3-carousel/dist/carousel.css';
import { Carousel, Slide, Navigation, Pagination } from 'vue3-carousel';
import { ref } from 'vue';

const props = defineProps(['url']);

const isLoading = ref(false);
const data = await fetch(`${props.url}/placeholder.json?limit=12`).then((response) => {
    const json = response.json();
    isLoading.value = false;
    
    return json;
})
</script>

<template>
    <Carousel v-if="!isLoading" :items-to-show="4" snap-align="center" :wrap-around="true">
        <Slide v-for="item in data?.items" :key="item.id">
            <div class="carousel__item mx-2">
                <h1 class="text-xl font-extrabold tracking-tight text-gray-900 mb-4">
                    {{ item.name }}
                </h1>

                <p class="text-zinc-500 text-sm">
                    {{ item.shortDescription }}
                </p>
            </div>
        </Slide>

        <template #addons>
            <Navigation></Navigation>
            <Pagination></Pagination>
        </template>
    </Carousel>
</template>
