<template>
    <div class="banner">
        <p>지금 결제하면 {{ discountRate }}% 할인!!</p>
    </div>
</template>

<script>
export default {
    name: 'Banner',
    data() {
        return {
            discountRate: 10,
        }
    },
    mounted() {
        let interval = setInterval(() => {
            if (this.discountRate > 0) {
                console.log("await..")
                this.discountRate--;
            } else {
                console.log("stopped")
                clearInterval(interval);
            }
        }, 1000);
    },
}
</script>

<style>
/* banner */
.banner {
    margin-bottom: 20px;
    padding: 20px;
    color: #FFF;
    font-weight: bold;
    border-radius: 16px;
    background-color: #2049A8;
}
</style>