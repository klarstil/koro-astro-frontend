<script setup lang="ts">
import BaseSlider from '../components/BaseSlider.vue';
import { getFormattedPrice } from '@shopware-pwa/helpers-next';
import { useProductSearch, useProduct } from '@shopware-pwa/composables-next';

const { search } = useProductSearch();

const props = defineProps<{
    id: string | undefined,
    url: string
}>();

const productResponse = await search(props.id as string);
const { product } = useProduct(productResponse.product);

const images = [
    'https://koro.imgix.net/media/fe/ef/95/1665569185/MUS_005_NEU_01_AK (2).jpg?auto=format,compress&w=240&h=380',
    'https://koro.imgix.net/media/f1/a2/c1/1665569186/MUS_005_NEU_05_AK (2).jpg?auto=format,compress&w=240&h=380',
    'https://koro.imgix.net/media/10/69/aa/1665569191/MUS_005_NEU_03_AK (1).jpg?auto=format,compress&w=240&h=380',
    'https://koro.imgix.net/media/e8/4a/c7/1665569244/MUS_005_NEU_08_AK.jpg?auto=format,compress&w=240&h=380',
    'https://koro.imgix.net/media/2b/3b/04/1665569248/MUS_005_NEU_06_AK (1).jpg?auto=format,compress&w=240&h=380',
    'https://koro.imgix.net/media/99/e5/e4/1665569248/MUS_005_NEU_07_AK (1).jpg?auto=format,compress&w=240&h=380',
    'https://koro.imgix.net/media/09/41/79/1665569249/MUS_005_NEU_02_AK.jpg?auto=format,compress&w=240&h=380',
];

const rawArguments = product.value.customFields.koro_buying_arguments as string;
const buyingArguments = rawArguments.split('\n');
</script>

<template>
    <div class="product-detail container mx-auto">
        <div class="flex flex-row flex-wrap justify-start">
            <div class="basis-8/12">
                <div class="columns-4 gap-5 [&>img:not(:first-child)]:mt-8 pt-4">
                    <img
                        v-for="(item, index) in images"
                        :key="index"
                        :src="item"
                        :alt="`White almond butter 500g Picture #${index}`"
                        width="240"
                        height="380"
                    />
                </div>
            </div>
            <div class="basis-4/12">
                <div class="p-4">
                    <h1 class="text-2xl font-extrabold tracking-tight text-gray-900 mb-4">
                        {{ product.name }}
                    </h1>

                    <p class="text-xl">
                        {{ getFormattedPrice(product.calculatedPrice.unitPrice, '€') }}
                    </p>

                    <ul v-if="buyingArguments.length" class="list-disc m-4">
                        <li v-for="(item, index) in buyingArguments" :key="index" class="mb-2 text-zinc-500">
                            {{ item }}
                        </li>
                    </ul>

                    <div class="flex flex-row">
                        <label for="quantitiy-select" class="sr-only">Quantity</label>
                        <input
                            id="quantitiy-select"
                            type="number"
                            min="1"
                            max="12"
                            class="bg-gray-50 border border-gray-300 text-gray-900 text-sm block p-2.5 mr-2"
                            value="1"
                        >
                        <button type="button" class="focus:outline-none text-white bg-green-700 block focus:ring-green-300 font-medium rounded text-sm p-2.5 w-full">
                            Add to shopping cart
                        </button>
                    </div>
                </div>
            </div>
        </div>

        <div class="my-8">
            <h1 class="text-2xl font-extrabold tracking-tight text-gray-900 mb-4">
                Client side only slider component
            </h1>
            <ClientOnly fallback-tag="div" fallback="Loading similar products...">
                <BaseSlider :url="url" client:only="vue"></BaseSlider>
            </ClientOnly>
        </div>
    </div>
</template>