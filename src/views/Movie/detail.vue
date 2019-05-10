<template>
    <div id="detailContrainer" class="slide-enter-active">
		<Header title="影片详情">
            <i class="iconfont icon-right" @touchstart="handleToBack"></i>
        </Header>
		<Loading v-if="loading"/>
		<div v-else id="content" class="contentDetail">
			<div class="detail_list">
				<div class="detail_list_bg" :style="{'background-image': 'url('+item.img.replace(/w\.h/,'148.208')+')'}"></div>
				<div class="detail_list_filter"></div>
				<div class="detail_list_content">
					<div class="detail_list_img">
						<img :src="item.img | setWH('140.208')">
					</div>
					<div class="detail_list_info">
						<h2>{{item.nm}}</h2>
						<p>{{item.enm}}</p>
						<p>{{item.sc}}</p>
						<p>{{item.cat}}</p>
						<p>{{item.src}} / {{item.dur}}分钟</p>
						<p>{{item.pubDesc}}</p>
					</div>
				</div>
			</div>
			<div class="detail_intro">
				<p>{{item.dra}}</p>
			</div>
			<div class="detail_player swiper-container">
				<ul class="swiper-wrapper">
					<li v-for="(photo,key) in item.photos" :key="key" class="swiper-slide">
						<div>
							<img :src="photo | setWH('230.273')" >
						</div>
					</li>
				</ul>
			</div>
		</div>
	</div>
</template>

<script>
import Header from '@/components/Header';
export default {
    components : {
        Header
	},
	data(){
		return {
			item:{},
			loading:true,
		}
	},
    props:['mid'],
    methods:{
        handleToBack(){
            this.$router.back();
        }
    },
    mounted(){
		console.log(this.mid);
		this.axios.get('/api/detailmovie?movieId='+this.mid).then((res)=>{
			console.log(res);
			//return;
            if(res.status===200){
				this.item = res.data.data.detailMovie;
				this.loading=false;
				this.$nextTick(()=>{
					new Swiper('.detail_player' , {
						slidesPerView : 'auto',
						freeMode : true,
						freeModeSticky: true
					});
				});
				
            }
        });
    }
}
</script>

<style scoped>
#detailContrainer{ position: absolute; left:0; top:0; z-index: 100; width:100%; min-height:100%; background:white;}
#detailContrainer.slide-enter-active{ animation:.1s slideMove;}
@keyframes slideMove{
    0%{ transform : translateX(100%); }
    100%{ transform : translateX(0); }
}
#content.contentDetail{ display: block; margin-bottom:0;}
#content .detail_list{ height:200px; width:100%; position: relative; overflow: hidden;}
.detail_list .detail_list_bg{ width:100%; height:100%; background: 0 40%; filter: blur(10px); background-size:cover; position: absolute; left: 0; top: 0;}
.detail_list .detail_list_filter{ width:100%; height:100%; position: absolute;background-color: #40454d;opacity: .55; position: absolute; left: 0; top: 0; z-index: 1;}
.detail_list .detail_list_content{ display: flex; width:100%; height:100%; position: absolute; left: 0; top: 0; z-index: 2;}
.detail_list .detail_list_img{ width:108px; height: 150px; border: solid 1px #f0f2f3; margin:20px;}
.detail_list .detail_list_img img{ width:100%; height: 100%;}
.detail_list .detail_list_info{ margin-top: 20px;}
.detail_list .detail_list_info h2{ font-size: 20px; color:white; font-weight: normal; line-height: 40px;}
.detail_list .detail_list_info p{ color:white; line-height: 20px; font-size: 14px; color:#ccc;}
#content .detail_intro{ padding: 10px;}
#content .detail_player{ margin:20px;}
.detail_player .swiper-slide{ width:70px; margin-right: 20px; text-align: center; font-size: 14px;}
.detail_player .swiper-slide img{ width:100%; margin-bottom: 5px;}
.detail_player .swiper-slide p:nth-of-type(2){ color:#999;}
</style>
