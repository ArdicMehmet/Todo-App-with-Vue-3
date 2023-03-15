<template>
    <h1 class="text-center mb-2">Todo List</h1>
    <input type="text" class="w-100 mb-2" @keydown.enter="addItem" v-model="provideItem.updateItem.content">
</template>
<script>
export default {
    name:"Header",
    inject:['addItem','provideItem']
}
</script>