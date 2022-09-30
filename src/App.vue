<template>
  <div id="wrap">
    <header>
      <ul class="gnb">
        <li v-for="item in gnbs" :key="item"><a href="#none">{{ item }}</a></li>
      </ul>
    </header>
    <main>
			<Banner v-if="showBanner == true" />
			<div class="sort-wrap">
				<button @click="priceSortDown">가격 ↓</button>
				<button @click="priceSortUp">가격 ↑</button>
				<button @click="charSortDown">문자 ↓</button>
				<button @click="charSortUp">문자 ↑</button>
				<button @click="sortBack">초기화</button>
			</div>
      <div class="card-list">
        <Card :oneroom="onerooms[i]" v-for="(item, i) in onerooms" :key="i" @openModal="modalView = true; modalContent = $event;" /><!-- 수신:  @openModal="" -->
      </div>
      <div class="video-wrap">
				<video loop="" autoplay="" muted="muted" playsinline=""><source src="https://onnuristorage.s3.ap-northeast-2.amazonaws.com/aftercare/contents/won_logo_movie.mp4" type="video/mp4"></video>
			</div>
    </main>
    <footer>
      <p>copyright by Liam</p>
    </footer>
		<transition name="fade">
			<Modal :onerooms="onerooms" :modalContent="modalContent" :modalView="modalView" @closeModal="modalView = false;" /><!-- 수신:  @closeModal="" -->
		</transition>
	</div>
</template>

<script>
import Banner from './components/Banner.vue';
import Modal from './components/Modal.vue';
import Card from './components/Card.vue';
import RoomData from './assets/oneroom.js';

//자식컴포넌트가 부모가 갖고 있는 데이터 쓰려면 props로 데이터를 전송해야 함
//1. 보내고, 2. 등록하고, 3. 사용

export default {
  name: 'App',
  data() {
    return {
      showBanner: true,
			oneroomsOrgin: [...RoomData], //deep capy
      gnbs: ['Home', 'About', 'Product', 'News'],
      onerooms: RoomData,
      modalContent: 0,
      modalView: false,
    }
  },
  methods: {
		//초기화
		sortBack() {
			this.onerooms = [...this.oneroomsOrgin];
		},
		//가격 ↓
		priceSortDown() {
			this.onerooms.sort(function(a, b){
				return a.price - b.price;
			});
		},
		//가격 ↑
		priceSortUp() {
			this.onerooms.sort(function(a, b){
				return b.price - a.price;
			});
		},
		//문자 ↓
		charSortDown() {
			this.onerooms.sort(function(a, b){
        if(a.title > b.title) return 1;
        if(a.title < b.title) return -1;
        if(a.title === b.title) return 0;
			});
		},
		//문자 ↑
		charSortUp() {
			this.onerooms.sort(function(a, b){
        if(a.title < b.title) return 1;
        if(a.title > b.title) return -1;
        if(a.title === b.title) return 0;
			});
		},
  },
  created() {
    
  },
  mounted() {

  },
  components: {
		Banner: Banner,
		Modal: Modal,
		Card: Card,
  },
}
</script>

<style>
/* transition */
.fade-enter-from {
	opacity: 0;
	transform: rotate(12deg) scale(1.6);
}
.fade-enter-active {
	transition: all .4s ease-in-out;
}
.fade-enter-to {
	opacity: 1;
	transform: rotate(0deg) scale(1);
}

.fade-leave-from {
	opacity: 1;
	transform: rotate(0deg) scale(1);
}
.fade-leave-active {
	transition: all .4s ease-in-out;
}
.fade-leave-to {
	opacity: 0;
	transform: rotate(12deg) scale(1.6);
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
ul, li {
  list-style: none;
}
em {
  font-style: normal;
}
a {
  color: #2c3e50;
  text-decoration: none;
}
img {
  width: 100%;
}
button {
  cursor: pointer;
}
video {
	width: 100%;
}
/* layout */
#wrap {
  display: flex;
  flex-direction: column;
  height: 100%;
  /* min-height: 100vh; */
}
header {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  background-color: #2C3639;
	z-index: 99;
}
main {
  margin-top: 60px;
  padding: 40px 20px 60px;
}
footer {
  margin-top: auto;
  padding: 20px;
  color: #2C3639;
  font-size: 11px;
  background-color: #DCD7C9;
}
/* gnb */
.gnb {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 20px;
}
.gnb a {
  display: block;
  padding: 21px 5px;
  color: #fff;
  opacity: .8;
}
.gnb a:hover {
  opacity: 1;
}

.card-list {
  display: flex;
  flex-wrap: wrap;
  margin: 0 -5px;
}
.card-list.col2 .card-box {
  flex-basis: calc(50% - 10px);
}

/* card-box */
.card-box {
	position: relative;
  overflow: hidden;
  display: block;
  width: 100%;
  margin-top: 40px;
  margin-left: 5px;
  margin-right: 5px;
  padding-bottom: 20px;
  border: 1px solid #e5e5e5;
  border-radius: 16px;
	background-color: #fff;
  box-shadow: 2px 8px 16px rgba(0,0,0,.15);
	transform: rotate3d(1, 1, 1, 0deg) scale(1);
	transition: all .3s ease;
	z-index: 1;
}
.card-box:hover {
	transform: rotate3d(1, 1, 1, 3deg) scale(1.03);
	z-index: 2;
}
.card-box .img {
  margin-bottom: 20px;
}
.card-box .tit {
  color: #3F4E4F;
  font-size: 16px;
}
.card-box .txt {
  margin-top: 6px;
  font-size: 14px;
}
.card-box .inp {
  margin-top: 15px;
}
.card-box .inp input {
	padding: 5px 10px;
	border: 1px solid #e5e5e5;
	border-radius: 4px;
}
.card-box .price {
  margin-top: 10px;
  font-size: 14px;
}
.card-box .price em {
  color: #A27B5C;
  font-size: 20px;
  font-weight: bold;
}
/* modal */
.modal {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0,0,0,.7);
  z-index: 9;
}
.modal .popup {
  position: absolute;
  top: 50%;
  left: 50%;
  width: 90%;
  max-width: 760px;
  background: #fff;
  transform: translate(-50%, -50%);
  z-index: 10;
}
.modal .popup.card-box .tit {
  font-size: 22px;
}
.modal .popup.card-box .price {
  margin-top: 20px;
  font-size: 16px;
}
.modal .popup.card-box .price em {
  font-size: 26px;
}
.btn-close {
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 20px auto 0;
  padding: 5px 20px;
  color: #fff;
  border: 1px solid #2C3639;
  border-radius: 8px;
  background-color: #2C3639;
}

.sort-wrap {
  display: flex;
  align-items: center;
  justify-content: center;
}
.sort-wrap button {
  display: flex;
  align-items: center;
  justify-content: center;
  white-space: nowrap;
  margin: 0 2px;
  padding: 5px 12px;
  color: #fff;
  border: 1px solid #222;
  border-radius: 8px;
  background-color: #222;
}

.video-wrap {
	overflow: hidden;
	position: relative;
	width: 100%;
	margin-top: 60px;
	padding-bottom: 100%;
	border-radius: 16px;
}
.video-wrap video {
	position: absolute;
	top: 50%;
	left: 50%;
	transform: translate(-50%, -50%);
	object-fit: cover;
}
</style>
