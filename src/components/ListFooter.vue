<template>
    <p class="text-center text-warning"> Görev sayısı : {{ todoItemCounter }}</p>
</template>

<script>
    export default {
        name:"ListFooter",
        props:['provideItem'],
        computed :{
            todoItemCounter(){
                return this.provideItem.list.length;
            }
        }
    }
</script>
