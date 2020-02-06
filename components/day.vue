<template>
    <div class="background">
    <div id="clouds" style="background:-webkit-linear-gradient(top, #c9dbe9 0%, #fff 100%">
    <div class="cloud x1"></div>
    <div class="cloud x2"></div>
    <div class="cloud x3"></div>
    <div class="cloud x4"></div>
    <div class="cloud x5"></div>
	</div>
    </div>
</template>

<script>
    export default {
        
    }
</script>

<style scoped>
*{ margin: 0; padding: 0;}

.background {
	height: 215vh;
	position: absolute;
	width: 500%
}

body {
	overflow: hidden;
}

#clouds{
	padding: 100px 0;
  transition: background 1s ease-out;  
}

.cloud {
	width: 200px; height: 60px;
	background-color: #fff;
	
	border-radius: 200px;
	-moz-border-radius: 200px;
	-webkit-border-radius: 200px;
	
	position: relative; 
}

.cloud:before, .cloud:after {
	content: '';
	position: absolute; 
	background: #fff;
	width: 100px; height: 80px;
	position: absolute; top: -15px; left: 10px;
	
	border-radius: 100px;
	-moz-border-radius: 100px;
	-webkit-border-radius: 100px;
	
	-webkit-transform: rotate(30deg);
	transform: rotate(30deg);
	-moz-transform: rotate(30deg);
}

.cloud:after {
	width: 120px; height: 120px;
	top: -55px; left: auto; right: 15px;
}

.x1 {
	left: -250px;

 	-webkit-transform: scale(0.4);
	-moz-transform: scale(0.4);
	transform: scale(0.4);

	-webkit-animation: moveclouds 120s linear infinite;
	-moz-animation: moveclouds 120s linear infinite;
	-o-animation: moveclouds 120s linear infinite;
}


.x2 {
	right:70px; top:-100px;
	
 	-webkit-transform: scale(0.5);
	-moz-transform: scale(0.5);
	transform: scale(0.5);
	opacity: 0.7; 	

	-webkit-animation: moveclouds 140s linear infinite;
	-moz-animation: moveclouds 140s linear infinite;
	-o-animation: moveclouds 140s linear infinite;
}

.x3 {
	left: -550px; top: -200px;
	
	-webkit-transform: scale(0.4);
	-moz-transform: scale(0.4);
	transform: scale(0.4);
	opacity: 0.8; 
	
	-webkit-animation: moveclouds 150s linear infinite;
	-moz-animation: moveclouds 150s linear infinite;
	-o-animation: moveclouds 150s linear infinite;
}

.x4 {
	left: 400px; top: -250px;
	
	-webkit-transform: scale(0.6);
	-moz-transform: scale(0.6);
	transform: scale(0.6);
	opacity: 0.75;
	
	-webkit-animation: moveclouds 100s linear infinite;
	-moz-animation: moveclouds 100s linear infinite;
	-o-animation: moveclouds 100s linear infinite;
}

.x5 {
	left: -750px; top: -250px;
	
	-webkit-transform: scale(0.47);
	-moz-transform: scale(0.47);
	transform: scale(0.47);
	opacity: 0.8;
	
	-webkit-animation: moveclouds 110s linear infinite;
	-moz-animation: moveclouds 110s linear infinite;
	-o-animation: moveclouds 110s linear infinite;
}

@-webkit-keyframes moveclouds {
	0% {margin-left: 1000px;}
	100% {margin-left: -1000px;}
}
@-moz-keyframes moveclouds {
	0% {margin-left: 1000px;}
	100% {margin-left: -1000px;}
}
@-o-keyframes moveclouds {
	0% {margin-left: 1000px;}
	100% {margin-left: -1000px;}
}

</style>