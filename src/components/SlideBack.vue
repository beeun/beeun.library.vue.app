<template>
	<div class="slide-back-wrap" v-on:touchstart="bodyTouchStart" v-on:touchmove="bodyTouchMove" v-on:touchend="bodyTouchEnd">
		<transition :name="direction">
			<slot></slot>
		</transition>
	</div>
</template>

<script>
/*
 * 右滑返回
 */
var swidth = document.documentElement.clientWidth;
export default {
	name: '',
	data() {
		return {
			// direction 页面切换的过渡动画，配合transition组件使用
	        direction: "slide-left",
	        // touchLeft 划动起点界限，起点在靠近屏幕左侧时才有效
	        touchLeft: swidth*2/5,
	        // touchStartPoint 记录起始点X坐标
	        touchStartPoint: 0,
	        // distance 记录划动的距离
	        distance: 0,
	        // 根据当前路由的配置来判断该路由是否可以回退，通过配置router的meta属性，unreturn = true表示禁止返回
	        unreturn: false,
	        historyName: ''
		}
	},
	mounted() {
		
	},
	watch: {
        // 监听路有变化，决定页面过渡动画
        $route(to, from) {
        	// 获取历史路由缓存
           	let routers = sessionStorage.getItem('routers');
           	if(routers) {
           		routers = routers.split(',');
           	}else{
           		routers = [];
           	}
           	// 判断是否是历史路由
           	if(routers.length == 0) {
           		routers.push(from.path)
        		routers.push(to.path);
           	}else{
           		if(routers.indexOf(to.path) != -1){
		          this.direction = 'slide-right';
		          routers.splice(routers.indexOf(to.path) + 1, 100);
		        }else{
		          this.direction = 'slide-left';
		          routers.push(to.path);
		        }
           	}
           	sessionStorage.setItem('routers', routers.join(','));
        }
    },
	methods: {
		bodyTouchStart: function(event) {
			const route = this.$route;
			let unreturn = false;
			if(route.meta && route.meta.unreturn) {
				unreturn = route.meta.unreturn;
			}
			if (!unreturn) {
                // 获得起点X坐标，初始化distance为0
                this.touchStartPoint = event.targetTouches[0].pageX;
                this.distance = 0;
            }
			this.unreturn = unreturn;
        },
        bodyTouchMove: function(event) {
        	if(!this.unreturn && this.touchStartPoint < this.touchLeft) {
        		// 只监听单指划动，多指划动不作响应
                if (event.targetTouches.length > 1) {
                    return;
                }
                // 实时计算distance
                this.distance = event.targetTouches[0].pageX - this.touchStartPoint;
                /*// 根据distance在页面上做出反馈。这里演示通过返回按钮的背景变化作出反馈
                if (this.distance > 0 && this.distance < 100) {
                    this.backBtn.style.backgroundPosition = ((this.distance - 100) / 100) * 50 + "px 0";
                } else if (this.distance >= 100) {
                    this.backBtn.style.backgroundPosition = "0 0";
                } else {
                    this.backBtn.style.backgroundPosition = "-50px 0";
                }*/
        	}
        },
        bodyTouchEnd: function(event) {
            if (!this.unreturn && this.touchStartPoint < this.touchLeft) {
                // 划动结束，重置数据
                this.touchStartPoint = 0;
                // this.backBtn.style.backgroundPosition = "-50px 0";
                // 当划动距离超过100px时，触发返回事件
                if (this.distance > 100) {
                    // 返回前修改样式，让过渡动画看起来更快
                    document.getElementById("app").classList.add("quickback");
                    this.$router.back();
                    setTimeout(function(){
                        document.getElementById("app").classList.remove("quickback");
                    },250)
                }
            }
        }
	}
}
</script>

<style>
.slide-back-wrap {
	min-height: 100vh;
}
.appView {
    position: absolute;
    width: 100%;
    background: #fff;
    min-height: 100vh;
    transition: transform .24s ease-out;
}
#app.quickback .appView{
    transition-duration: .2s;
}
.slide-left-enter {
    transform: translate(100%, 0);
}
.slide-left-leave-active {
    transform: translate(-50%, 0);
}
.slide-right-enter {
    transform: translate(-50%, 0);
}
.slide-right-leave-active {
    transform: translate(100%, 0);
}
</style>