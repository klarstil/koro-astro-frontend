<script setup lang="ts">
import BaseProductCard from '../components/BaseProductCard.vue';
import { toRef } from 'vue';
import { useCategorySearch, useCategory, useListing } from '@shopware-pwa/composables-next';

const { search } = useCategorySearch();
const response = await search('41b852bf83134c8a9a9b89fea74b023c', {
    withCmsAssociations: true,
});

const categoryResponse = toRef(response);

const { category } = useCategory(categoryResponse);
const listing = category.value.cmsPage?.sections[0].blocks[3].slots[0];
const {
    getElements,
    setInitialListing,
} = useListing({
    listingType: 'categoryListing',
    defaultSearchCriteria: {
        p: 1,
        limit: 12,
    },
});

// @ts-ignore
setInitialListing(listing?.data?.listing);
</script>

<template>
    <div class="container mx-auto pt-4">
        <div class="grid grid-cols-1 gap-4 md:grid-cols-4">
            <BaseProductCard v-for="item in getElements" :key="item.id" :product="item"></BaseProductCard>
        </div>
    </div>
</template>