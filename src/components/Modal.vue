<template>
    <div class="modal" v-if="modalView == true">
        <div class="popup card-box" >
            <p class="img"><img :src="onerooms[modalContent].image"></p>
            <h4 class="tit">{{ onerooms[modalContent].title }}</h4>
            <p class="txt">{{ onerooms[modalContent].content }}</p>
            <!-- <p><input @input="month = $event.target.value"> 개월</p> -->
            <p class="inp"><input v-model="month"> 개월</p>
            <p class="price">{{ month }} 개월 선택함 : <em>{{ onerooms[modalContent].price * month }}</em> 만원</p>
            <button type="button" class="btn-close" @click="modalControl"><span>닫기</span></button><!-- 발신: $emit('closeModal') -->
        </div>
    </div>  
</template>

<script>
export default {
    name: 'Modal',
    data() {
        return {
            month: 1,
        }
    },
    watch: {
        month(n) {
            if (n > 12) {
                alert('12개월 이상 안됨');
                this.month = 12;
            }
            // if (n < 1) {
            //     alert('1개월 이상 입력');
            //     this.month = 1;
            // }
            if (isNaN(n)) {
                alert('숫자만 입력');
                this.month = 1;
            }
            console.log(this.month);
        },
    },
    beforeUpdate() {
        if(this.month <= 2) {
            alert('2개월 이상 입력');
            this.month = 3;
        }
        console.log(this.month);
    },
    props: {
        onerooms: Array,
        modalContent: Number,
        modalView: Boolean,
    },
    methods: {
        modalControl() {
            this.$emit('closeModal');
        } 
    }
}
</script>

<style>

</style>