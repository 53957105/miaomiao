<template>
    <div class="city_body">
				<div class="city_list">
					<div class="city_hot">
						<h2>热门城市</h2>
						<ul class="clearfix">
							<li v-for="item in list.hotList" :key="item.id">{{item.nm}}</li>
						</ul>
					</div>
					<div class="city_sort" ref="city_sort">
						<div v-for="items in list.cityList" :key="items.index">
							<h2>{{items.index}}</h2>
							<ul>
								<li v-for="item in items.list" :key="item.id">{{item.nm}}</li>
							</ul>
						</div>
					</div>
				</div>
				<div class="city_index">
					<ul>
						<li v-for="(items,index) in list.cityList" :key="items.index" @click="toIndex(index)">{{items.index}}</li>
					</ul>
				</div>
			</div>
</template>

<script>
export default {
	name:'City',
	data(){
		return {
			list:{
				cityList:[],
				hotList:[]
			}
		}
	},
	mounted(){
		this.axios.get('/api/cityList').then((res)=>{
			if(res.status===200){
				this.list=this.formatCityList(res.data.data.cities);
				
			}
		});
	},
	methods:{
		formatCityList(cities){
			let tmp=[];
			let cityList=[];
			let hotList=[];
			for (let i = 0; i < 26; i++) {
				let char=String.fromCharCode(65+i);
				tmp[char]={list:[]};
			}
			cities.forEach(element => {
				let firstLetter=element.py.substring(0,1).toUpperCase();
				tmp[firstLetter].list.push(element);
				//console.log(element);
				if(element.isHot===1){
					hotList.push(element);
				}
			});
			for (let i = 0; i < 26; i++) {
				let char=String.fromCharCode(65+i);
				if(tmp[char].list.length>0){
					tmp[char].index=char;
					cityList.push(tmp[char]);
				}
			}
			return{cityList,hotList};
		},
		toIndex(index){
			var h2=this.$refs.city_sort.getElementsByTagName('h2');
			this.$refs.city_sort.parentNode.scrollTop=h2[index].offsetTop;
		}
	}
}
</script>

<style scoped>
#content .city_body{ margin-top: 45px; display: flex; width:100%; position: absolute; top: 0; bottom: 0;}
.city_body .city_list{ flex:1; overflow: auto; background: #FFF5F0;}
.city_body .city_list::-webkit-scrollbar{background-color:transparent;width:0;}
.city_body .city_hot{ margin-top: 20px;}
.city_body .city_hot h2{ padding-left: 15px; line-height: 30px; font-size: 14px; background:#F0F0F0; font-weight: normal;}
.city_body .city_hot ul li{ float: left; background: #fff; width: 29%; height: 33px; margin-top: 15px; margin-left: 3%; padding: 0 4px; border: 1px solid #e6e6e6; border-radius: 3px; line-height: 33px; text-align: center; box-sizing: border-box;}
.city_body .city_sort div{ margin-top: 20px;}
.city_body .city_sort h2{ padding-left: 15px; line-height: 30px; font-size: 14px; background:#F0F0F0; font-weight: normal;}
.city_body .city_sort ul{ padding-left: 10px; margin-top: 10px;}
.city_body .city_sort ul li{ line-height: 30px; line-height: 30px;}
.city_body .city_index{ width:20px; display: flex; flex-direction:column; justify-content:center; text-align: center; border-left:1px #e6e6e6 solid;}
</style>
