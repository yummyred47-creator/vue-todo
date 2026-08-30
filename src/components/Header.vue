<script setup>
    const isShowIf = true;
    const isShowElseIf = false;

</script>
<template>
    <h1 v-if="isShowIf">Is v-if</h1>
    <h1 v-else-if="isShowElseIf">Is v-else-if</h1>
    <h1 v-else="isShowElse">Is v-else</h1>

    <h1 v-show="isShowIf">Is v-show</h1>
</template>