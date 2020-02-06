<template>
  <div class="background">
    <div id="layers">
      <div class="layer"></div>
      <div class="layer"></div>
      <div class="layer"></div>
      <div class="layer"></div>
      <div class="layer"></div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'night',
  created(){
    this.gettime()
  },
  data(){
    return {
    }
  },
  methods:{
    gettime(){
      var currentDate = new Date();
      console.log(currentDate);
      var currentDateWithFormat = new Date().toJSON().slice(0,10).replace(/-/g,'/');
      console.log(currentDateWithFormat);
    }
    

  }
}
</script>

<style scoped>
.background{
	background: linear-gradient(0deg, #383d95, #040d30 90%) no-repeat #383d95;
  margin: 0;
  height: 215vh;
  position: absolute;
	width: 200%
}
#layers {
	height: 200px;
	left: 0;
	overflow: hidden;
	position: absolute;
	top: 0;
	width: 100vw;
}
#layers .layer {
	background-size: auto 100%;
	background-position: 50% 0;
	background-repeat: repeat-x;
	height: 200px;
	left: 0;
	position: absolute;
	top: 0;
	transform: translate3d(0, 0, 0);
	width: 200vw;
}
@keyframes twinkle {
	from { opacity: .3; }
	to { opacity: 1; }
}
@keyframes layer-1 {
	from { transform: translate3d(0, 0, 0); }
	to { transform: translate3d(-689px, 0, 0); }
}
#layers .layer:nth-child(1) {
	animation: twinkle 10s ease-in -5s infinite alternate, layer-1 344s linear infinite;
	background-image: url('http://imgur.com/9beLKl9.png');
}
@keyframes layer-2 {
	from { transform: translate3d(0, 0, 0); }
	to { transform: translate3d(-699px, 0, 0); }
}
#layers .layer:nth-child(2) {
	animation: twinkle 3s ease-in -3s infinite alternate, layer-2 349s linear infinite;
	background-image: url('http://imgur.com/46MRYFa.png');
}
@keyframes layer-3 {
	from { transform: translate3d(0, 0, 0); }
	to { transform: translate3d(-699px, 0, 0); }
}
#layers .layer:nth-child(3) {
	animation: twinkle 3.5s ease-in infinite alternate, layer-3 349s linear infinite;
	background-image: url('http://imgur.com/46MRYFa.png');
}
@keyframes layer-4 {
	from { transform: translate3d(0, 0, 0); }
	to { transform: translate3d(-480px, 0, 0); }
}
#layers .layer:nth-child(4) {
	animation: twinkle 4s ease-in -2s infinite alternate, layer-4 240s linear infinite;
	background-image: url('http://imgur.com/BqkETyl.png');
}

@keyframes layer-5 {
	from { transform: translate3d(0, 0, 0); }
	to { transform: translate3d(-486px, 0, 0); }
}
#layers .layer:nth-child(5) {
	animation: twinkle 5.5s ease-in infinite alternate, layer-5 243s linear infinite;
	background-image: url('http://imgur.com/x1GB77K.png');
}

@keyframes shooting-star {
	0% {
		opacity: 1;
		transform: translate3d(0, 0, 0);
	}
	25% {
		opacity: 1;
	}
	100% {
		opacity: 0;
		transform: translate3d(-270px, 180px, 0);
	}
}

#layers:before {
	animation: 500ms linear 3s shooting-star;
	background-image: url('http://imgur.com/ilhrli3.png');
	background-size: 72px 50px;
	content: ' ';
	height: 50px;
	left: 80%;
	position: absolute;
	top: -50px;
	width: 72px;
	z-index: 100;
}

</style>
