<template>
    <div class="max-w-md mx-auto overflow-hidden bg-white rounded-lg shadow-lg my-3 hover:animate-pulse ">
        <div class="px-4 py-2">
            <h1 class="text-base font-bold text-gray-800 uppercase text-center">
                <slot name="header" ></slot>
            </h1>
        </div>
        <slot name="image"></slot>
        <div class="flex items-center justify-between px-4 py-2 bg-white">
            <slot name="content"></slot>
        </div>
    </div>
</template>

<script>
    export default {

    }

</script>

<style lang="scss" scoped>

</style>
