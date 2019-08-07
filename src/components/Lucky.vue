<template>
	<div class="lucky">
		<canvas class="can" ref="can" width="1200" height="600"></canvas>
		<canvas ref="heartCan" width=50 height=60 style="display:none;"></canvas>
	</div>	
</template>

<script>
import { random } from 'lodash-es';

const HEART_COUNT = 20;
const HEART_MOVE_TIME_LOW_LIMIT  = 1000;
const HEART_MOVE_TIME_HIGH_LIMIT  = 1500;

function drawHeart(ctx,x,y,R,rot,color) { 
	function heartPath(ctx) { 
		ctx.beginPath(); 
		ctx.arc(-1,0,1,Math.PI,0,false); 
		ctx.arc(1,0,1,Math.PI,0,false); //貝塞尔曲线画心 
		ctx.bezierCurveTo(1.9, 1.2, 0.6, 1.6, 0, 3.0); 
		ctx.bezierCurveTo( -0.6, 1.6,-1.9, 1.2,-2,0); 
		ctx.closePath(); 
	}
    ctx.save(); 
    ctx.translate(x,y); 
    ctx.rotate(rot/180*Math.PI); 
    ctx.scale(R, R); 
    heartPath(ctx);
	ctx.fillStyle = color; 
	ctx.fill(); 
} 

function startDrawTextAndHeart(container, heartCan, y, x, texts) {
	return new Promise(resolve => {
		const textAnimInfos = texts.split('').map((item, index) => {
			let text = new createjs.Text(item, "50px monospace", "#000000");
			text.x = x + index * 60;
			text.y = y;
			text.rotation = random( -30, 30);
			text.scale = random(1.2, 1.5);
			return text;
		});
		let i = 0;
		function renderText() {
			const text = textAnimInfos[i];
			container.addChild(text);
			createjs.Tween.get(text).to({rotation: random(-10, 10), scale: random(0.8, 1)}, 100).call(() => {
				if(i >= texts.length) resolve();
			});

			i++;    
			if (i < texts.length) {
				setTimeout(renderText, 350); 
			}          
		}

		let j =0;
		function renderHeart() {
			const text = textAnimInfos[j];
			for (let i =0; i< HEART_COUNT; i++){
				const startX =text.x + random(0, 20);
				const startY =text.y + random(0, 20);
				var heart = new createjs.Bitmap(heartCan);
				heart.x = startX;
				heart.y = startY;
				heart.width = 10;
				heart.height = 10;
				heart.scale = random(0.5, 1);
				container.addChild(heart);
				createjs.Tween.get(heart).wait(0).to({y: random(-1200, 1200), x: random(-1200, 1200), rotatation: random(0, 360)}, random(HEART_MOVE_TIME_LOW_LIMIT, HEART_MOVE_TIME_HIGH_LIMIT));
			}

			j++;
			if (j < texts.length) {  
				setTimeout(renderHeart, 350); 
			}
		}

		setTimeout(() => {
			renderText();
			renderHeart();
		}, 2000);
	})
}
export default {
	name: 'Lucky',
	mounted() {
        const heartCan = this.$refs.heartCan;
        const heartCtx = heartCan.getContext('2d');
		drawHeart(heartCtx, 25,25,10, 0, '#ff0000');
	
		const can = this.$refs.can;

        const stage = new createjs.Stage(can);     
        const container = new createjs.Container();
		stage.addChild(container);

		(async () => {
			await startDrawTextAndHeart(container, heartCan, 200, 350, '一直没有明确告诉你');
			await startDrawTextAndHeart(container, heartCan, 300, 300, '遇见你是多么美好的事情');
		})();
		
		function tick(evt) {
			stage.update();
		}
		createjs.Ticker.addEventListener("tick", tick);
		createjs.Ticker.setFPS(30);
	}	
}
</script>

<style>
	body{
		/* background: #000; */
	}
</style>

<style scoped>
	.can {
		display:block;
		margin: 50px auto;
	}
</style>

