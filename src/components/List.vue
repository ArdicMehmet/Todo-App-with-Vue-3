<template>
    <ul class="list-group">
        <slot name="list" v-for="todoItem in provideItem.list" :todo="todoItem" ></slot>
    </ul>
</template>

<script>
    export default {
        name:"List",
        props:['provideItem'],
    }
</script>
