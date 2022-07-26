<template>
    <div class="ellipse" @click.stop="showModal">
        <img class="img-plus" :src="plus" alt=""/>
    </div>
</template>

<script>
export default {
    data() {
        return {
            plus: require('../assets/img/plus.svg'),
        }
    },
    methods: {
        showModal() {
            this.$emit('showModal');
        },
    },
}
</script>

<style lang="scss" scoped>
.ellipse{
    width:40px;
    height:40px;

    background-color:#D6DBEB;
    border-radius: 50%;
}
.img-plus{
    display:block;
    margin: 0 auto;
    margin-top: 25%;
}

</style>