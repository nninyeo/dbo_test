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
		asdf: 0,
    }
  },
  components: {
  },
  methods: {

	//마우스 누르면 이벤트 나는데 클라이언트가 마우스찍었을때 값.
	mouseLeft(event) {console.log(event)
		
		console.log("좌클릭됬음")
		//좌클릭 이벤트 발생시에 그 좌표(event.clientXY)를 originXY에 넣기
		this.originX = event.clientX;
		this.originY = event.clientY;

		//1. 여기서 누른걸 감지한다. 감지한걸 플래그에 넣는다.
		this.mouseState = true;
	},

	mouseUp(event) {
	
		//이벤트를 받아왔다.
		//이걸 왜 하냐면 한번 이동한후에 또 이동하면 값이털려서 다시 계산할려고 

		//우선 마우스 상태를 비활성화한다.
		this.mouseState = false;

		//받아온 이벤트에서 타겟(이벤트가 발생한 대상, 여기서는 백그라운드이미지 달린 div)의
		//전체 태그를 다가져온다 하위요소까지. 가져와서 그 태그들을 tg에 넣어버렸다
		const tg = event.target

		//getComputedStyle: 해당 엘리먼트의 스타일 목록 싹 뽑아오는거
		//아까 가져온태그안에서 스타일목록만 뽑아다가 styles에 박아버렸다.
		const styles = getComputedStyle(tg);
		//const쓴 이유는 그냥 이거 전용이니까

		//그러고나서 스타일 안에서 background-position-x 라는 놈을 뽑아오려는거다
		//getPropertyValue는 값 뽑아오는 것이다.
		const bgPosisionX = styles.getPropertyValue('background-position-x');
		const bgPosisionY = styles.getPropertyValue('background-position-y');
		//background-position-x를 뽑아다가 bgPosision에 넣어뒀다.
		//근데 이게 문자열이라서 int형으로 바꿀거다. 10은 10진수란소리다 혹시몰라쓴거다.


		//이제 드래그하고 나서 바뀐 background-position의 값이 originBgPosXY에 들어갔다.
		this.originBgPosX = parseInt(bgPosisionX,10);
		this.originBgPosY = parseInt(bgPosisionY,10);


		console.log(parseInt(styles.getPropertyValue('background-position-x'),10));

	},
		
	mouseMove(event) {
		//2. 누른건 감지했고 마우스 움직이면 지도가 따라간다
		//마우스 포인터값만큼 지도 bg어쩌구 값을빼준다
		if(this.mouseState){//애가 트루면 눌린거
			
			
			
			const x = event.clientX;
			const y = event.clientY;
			const tg = event.target;
			


			tg.style.backgroundPositionX = x - this.originX + this.originBgPosX + 'px';
			tg.style.backgroundPositionY = y - this.originY + this.originBgPosY + 'px';

			//console.log("origin" + this.originX)
			//console.log("originBgPosX" + this.originBgPosX)



		}

	},
		
	mouseRight(event) {
		this.asdf = event;
	},
		
	markerInit(event) {
		console.log("sd %o",event)
	},
		
	contextMenu(event) {
		this.asdf = event;
	}
  }

}
</script>