<template>
	<div id="shopdetails" style="position: relative;">
		<section class="main fixed" id="commDetailS" style="position: relative;height:calc(100vh - 3.26rem);overflow-x: hidden;overflow-y: auto;background-color:#fafafa;">
			<div class="con_top" style="background-color:#fff;">
				<a href="javascript:void(0)"><img v-bind:src="geturl+shopDetail.image" alt=""></a>
				<div class="info">
					<p>{{shopDetail.name}}</p>
					<span>积分兑换：{{shopDetail.point*50}}</span><span>配送费：0元</span>
					<span class="span2">￥{{shopDetail.price}}/{{shopDetail.unit}}</span>
				</div>
			</div>
			<div class="con_bottom" style="background-color:#fff;">
				<div class="con_bottom_title">详情说明</div>
				<div class="detali-img">
					<img src="../../images/restaurant/menu_detail_image@2x.png" alt="">
					<img class="back-bg-img" src="../../images/restaurant/bg_img@2x.png" alt="" />
				</div>
				<ul>
					<li>
						<p><span class="cy-li-name">品&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;名：</span><span class="cy-li-contont">重庆豆花烤鱼【凌波鱼】</span></p>
						<p><span class="cy-li-name">辣&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;度：</span><span class="cy-li-contont">中辣</span></p>
						<p><span class="cy-li-name">菜品特色：</span><span class="cy-li-contont">是长居销量榜top1的明星产品，原料中的贵州山区原生态辣椒、山胡椒、薄荷、鱼腥草根等，使得口味椒香麻辣、纯正浓厚。用盐卤点制的豆花更是不愿甘当配角，简直比烤鱼还要香嫰，十足一副喧宾得主的架势！</span></p>
						<p><span class="cy-li-name">提&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;示：</span><span class="cy-li-contont">吃不惯鱼腥草和薄荷叶的客户们记得告知服务员取消哦~</span></p>
					</li>
				</ul>
			</div>
			<!--<div class="buyCar">introduction
				<a href="javascript:void(0)"></a>
			</div>-->
		</section>
		<div class="detailFooter">
			<span class="bottom-money"><em>￥{{shopDetail.price}}</em>  元/{{shopDetail.unit}}</span>
			<span @click="payShoping()" class="buttom but-shoping" >加入购物车</span>
			<!--<span class="buttom but-pay">立即购买</span>-->
		</div>
	</div>
</template>
<script type="text/javascript">
	export default {
		data: function() {
			return {
				shopDetail: {},
				geturl:configuration.global.imgPath
			}
		},
		mounted: function() {
			this.shopDetail = JSON.parse(this.$route.query.shopDetail)
			globalMethod.setHscroll("commDetailS");
		},
		methods: {
			backPage: function() {
				if(window.history && window.history.pushState) window.history.back();
			},
			redCommodity: function() {
				var inputElement = document.getElementsByClassName("commodity-change-input")[0];
				inputElement.innerText = Number(inputElement.innerText) > 0 ? Number(inputElement.innerText) - 1 : 0;
				changeLocalStorage(false, this.shopDetail);
			},
			addCommodity: function() {
				var inputElement = document.getElementsByClassName("commodity-change-input")[0];
				inputElement.innerText = Number(inputElement.innerText) + 1;
				changeLocalStorage(true, this.shopDetail);
			},
			initNum: function(shopCart, shopDetail) {
				for(var i = shopCart.length; i--;) {
					if(shopDetail.id == shopCart[i].id) shopDetail.num = shopCart[i].num;
				}
				return shopDetail;
			},
			payShoping:function(){
				globalMethod.changeLocalStorage(true,this.shopDetail)
				this.$router.push({name:"shoppingHome"})
			}
		}
	}
</script>
<style>
	#shopdetails {
		background-color: #fff;
	}
	#shopdetails .detali-img{
		position: relative;
		width: 100%;
		height: auto;
	}
	
	.con_top {
		padding: 0.6666666666666666rem 0.6666666666666666rem 0 0.6666666666666666rem;
		border-bottom: 1px solid #E5E5E5;
		
	}
	
	.con_top a {
		display: block;
		width: 100%;
		height: 100%;
	}
	
	.con_top a img {
		width: 23.5rem;
		height: 17rem;
	}
	
	.con_top .info {
		padding-left: 0.13333333333333333rem;
	}
	
	.con_top .info p {
		margin-top: 0.5rem;
		font-size: 1rem;
		color: #303030;
		overflow: hidden;
		letter-spacing:0.08rem;
	}
	
	.con_top .info span {
		color: #666666;
		font-size: 0.9rem;
		overflow: hidden;
		
	}
	
	.con_top .info span:nth-of-type(1) {
		margin-right: 2.6666666666666665rem;
		line-height: 1.8rem;
	}

	.con_top .info span:nth-of-type(2){
		letter-spacing:0.06rem;
	}
	
	.con_top .info .span2 {
		display: block;
		font-size: 0.9333333333333333rem;
		color: #d5383e;
		margin-bottom:0.5rem;
		letter-spacing:0.04rem;
	}
	
	.con_bottom {
		padding: 0px 0.3rem 0 0.3rem;
		background: white;
		margin-top: 0.6666666666666666rem;
		margin-bottom: 1.5rem;
		border-top: 1px solid #E5E5E5;

	}
	
	.con_bottom .con_bottom_title {
		text-align: center;
		font-size: 1rem;
		height: 3.033333333333333rem;
		padding-top: 0.6rem;
		border-bottom: 1px solid #E5E5E5;
	}
	
	.con_bottom img {
		margin: 0.8rem 0 0.8rem 0;
		width: 100%;
		height: 8.9rem;
	}
	.con_bottom .back-bg-img{
		position: absolute;
		top: 0;
	}
	.con_bottom ul {
		padding-left: 0.6666666666666666rem;
	}
	
	.con_bottom ul li {
		color: #333333;
		font-size: 0.8rem;
		padding-bottom:0.8rem;
	}
	
	.con_bottom ul li p {
		font-size: 0.933rem;
		color: #333333;
		display: -webkit-flex;
		display: flex;
	}
	
	.con_bottom ul li p span em {
		padding-left: 1.6rem;
	}
	
	.con_bottom>ul>li>p .cy-li-name {
		text-align: left;
		width: 22%;
		letter-spacing:0.04rem;
	}
	
	.con_bottom>ul>li>p .cy-li-contont {
		width: 80%;
		letter-spacing:0.08rem;
	}
	
	.buyCar {
		width: 3.066666666666667rem;
		height: 3.066666666666667rem;
		position: fixed;
		right: 0.8rem;
		bottom: 3.6666666666666665rem;
		margin-block-end: 0.5666666666666667rem;
	}
	
	.buyCar a {
		display: block;
		width: 100%;
		height: 100%;
		background: url("../../images/restaurant/menu_add_shop_cart_img@2x.png") no-repeat center/cover;
	}
	/*底部*/
	.detailFooter {
		position: fixed;
		left: 0;
		bottom: 0;
		width: 100%;
		height: 3.26rem;
		background-color: #fff;
		border-top: 1px solid #E5E5E5;
		padding: 0 0.8rem;
		z-index: 999;
		display: -webkit-flex;
		display: flex;
		-webkit-align-items: center;
		align-items: center;
		-webkit-justify-content: flex-end;
		justify-content: flex-end;
	}
	
	.detailFooter .bottom-money {
		position: absolute;
		left: 0.8rem;
		font-size: 0.8rem;
		color: #666;
	    display: inline-block;
	    height: 3.26rem;
	    line-height: 3.26rem;	
	    	
	}
	
	.detailFooter .bottom-money>em {
		font-size: 1.2rem;
		color: #D5383E;
		font-weight: 600;
		letter-spacing:0.05rem;
	}
	
	.detailFooter .buttom {
		display: inline-block;
		border: 1px solid #f7353c;
		border-radius: 25px;
		font-size: 0.9333333333333333rem;
		padding: 0.4rem 0.7rem;
	}
	
	.detailFooter .but-shoping {
		color: #f7353c;
	}
	
	.detailFooter .but-pay {
		background-color: #D5383E;
		color: #FFF;
	}
</style>