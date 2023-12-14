<template>
    <div class="flex  w-full">
        <div class="min-w-[150px] md:min-w-[300px]">
            <Menu/>
        </div>
        <div class="p-2 flex-grow-1">
            <slot/>
        </div>
    </div>
</template>

<script setup>
import Menu from "@/Components/Menu.vue";


</script>