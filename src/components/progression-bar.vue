<script setup lang="ts">
defineProps(['progression'])
</script>

<template>

    <div class=" p-4 mt-2">
        <div class=""> <!-- Largeur ajustable -->
            <div class="flex mb-2 justify-between">
                <div>
                    <span
                        class="text-xs font-semibold inline-block py-1 px-2 uppercase rounded-full text-teal-600 bg-teal-200">
                        In Progress
                    </span>
                </div>
                <div class="text-right">
                    <span class="text-xs font-semibold inline-block text-teal-600">
                        {{ progression }}%
                    </span>
                </div>
            </div>
            <div class="flex rounded-full h-2 bg-gray-200">
                <div class="rounded-full bg-teal-500" :style="{ width: progression + '%' }"></div>
            </div>
        </div>
    </div>
</template>