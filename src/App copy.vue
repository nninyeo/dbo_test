<template>
  <div id="container" style="background-image: url(/map.png); width: 1024px; height: 768px; border: 1px solid black;"
    v-on="{ mouseup: mouseUp, mousemove: mouseMove }" @mousedown.left="mouseLeft" @mousedown.right="mouseRight" @contextmenu="contextMenu">
    <img src="/reset.png" style="position: absolute; top: 12px; left: 960px;" @click="markerInit" />
  </div>
</template>

<script>

export default {
  name: 'App',
  data(){
    return {
		originX: 0,
		originY: 0,
		originBgPosX: 0,
		originBgPosY: 0,
		mouseState: false,
		defaultMarkerX: [],
		defaultMarkerY: [],
    }
  },
  components: {
  },
  methods: {

	//마우스 누르면 이벤트 나는데 클라이언트가 마우스찍었을때 값.
		mouseLeft(event) {
			this.originX = event.clientX;
			this.originY = event.clientY;
			this.mouseState = true;
			console.log("originX = "+ this.originX);
			console.log("originX = "+ this.originX);
			console.log("event.clientX = "+ event.clientX);
			console.log("event.clientY = "+ event.clientY);




			//???
			const markerImage = document.querySelectorAll('[class=marker-image]');
			
			//재 초기화, 안하면 계속늘어남.
			this.defaultMarkerX = [];
			this.defaultMarkerY = [];

			//마커이미지만큼?
			for(const el of markerImage) {
				this.defaultMarkerX.push(Number(el.getAttribute('default-left').replace('px', '')));
				this.defaultMarkerY.push(Number(el.getAttribute('default-top').replace('px', '')));
			}

console.log(this.defaultMarkerX)
			//const tg = event.target
			//const styles = getComputedStyle(tg);
			//this.originBgPosX = parseInt(styles.getPropertyValue('background-position-x'),10);
		},

    mouseUp(event) {
			const tg = event.target
			const styles = getComputedStyle(tg);
			
			this.originBgPosX = parseInt(styles.getPropertyValue('background-position-x'),10);
			this.originBgPosY = parseInt(styles.getPropertyValue('background-position-y'),10);
			this.mouseState = false;
		},
		
		mouseMove(event) {
			if(!this.mouseState) return false;
      //마우스 좌클릭 후 드레그 시 마커, 리셋 아이콘 블록 잡히는 경우 블록해제
      if(window.getSelection) window.getSelection().removeAllRanges();
			
			const x = event.clientX;
			const y = event.clientY;
			const tg = event.target;
			
			if(x - this.originX + this.originBgPosX >= 0) return false;
			if(x - this.originX + this.originBgPosX <= -1883) return false;
			if(y - this.originY + this.originBgPosY >= 0) return false;
			if(y - this.originY + this.originBgPosY <= -2692) return false;
			
			tg.style.backgroundPositionX = x - this.originX + this.originBgPosX + 'px';
			tg.style.backgroundPositionY = y - this.originY + this.originBgPosY + 'px';
			
			const markerImage = document.querySelectorAll('[class=marker-image]');
			
			for(let i = 0; i < markerImage.length; i++) {
				markerImage[i].style.left = this.defaultMarkerX[i] + (x - this.originX + this.originBgPosX) + 'px';
				markerImage[i].style.top = this.defaultMarkerY[i] + (y - this.originY + this.originBgPosY) + 'px';
			}
		},
		
		mouseRight(event) {
			const x = event.clientX - 45;
			const y = event.clientY - 130;

			const marker = document.createElement('img');
			marker.setAttribute('style', `position: absolute; left: ${x}px; top: ${y}px;`);
			marker.setAttribute('default-left', `${x}px`);
			marker.setAttribute('default-top', `${y}px`);
			marker.className = 'marker-image';
			marker.src = '/marker.png';
			marker.addEventListener('mousedown', (event) => {
				event.stopPropagation();
				event.preventDefault();
			})
			marker.addEventListener('mousemove', (event) => {
				event.stopPropagation();
			})
			event.target.append(marker);
		},
		
		markerInit() {
			const markerImage = document.querySelectorAll('[class=marker-image]');
			for(const el of markerImage) el.remove();
		},
		
		contextMenu(event) {
			event.preventDefault();
		}
  }

}
</script>