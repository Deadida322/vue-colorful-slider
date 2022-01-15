<template>
    <div class="slide">
        <slot/>
    </div>
</template>

<script>
export default {
    
}
</script>

<style lang="css" scoped>
.slide{
    width: 100%;
    height: 100%;
    position: absolute;
    top: 0;
    transition: all 1s ease-in-out;
    left: 0;
}
</style>