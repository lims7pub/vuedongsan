<template>
    <div class="card-box">
        <p class="img" @click="modalControl"><img :src="oneroom.image"></p><!-- 발신: $emit('openModal') -->
        <h4 class="tit">{{ oneroom.title }}</h4>
        <p class="price"><em>{{ oneroom.price }}</em> 만원</p>
    </div>
</template>

<script>
export default {
    name: 'Card',
    props: {
        oneroom: Object,
    },
    methods: {
        modalControl() {
            this.$emit('openModal', this.oneroom.id);
        } 
    }
}
</script>

<style>

</style>