<template>
	<div id="shoppingHome" style="position: relative;">
		<section class="main fixed" id="has_shopping" style="background-color:#e5e5e5;">
			<article class="content" id="shopHomePage" style="position: relative;height:calc(100vh - 6.26rem);overflow-x: hidden;overflow-y: auto;">
				<div class="shoping-commoditys">
					<li v-for="item in shopOrder.order" >
						<div class="commodity-information comm-li-info">
							<div class="information-name">{{item.name}}</div>
							<div class="information-pice"><span class="infor-p-span">¥ {{item.price}}元/{{item.unit}}</span><span class="infor-j-span">积分    {{item.point}}</span></div>
						</div>
						<div class="commodity-choice">
							<div class="commodity-change-1 c-red1">-1</div>
							<div class="commodity-change-j" v-on:mousedown="redCommodity(item,$event)" ></div>
							<div class="commodity-change-input">{{item.num}}</div>
							<div class="commodity-change-z" v-on:mousedown="addCommodity(item,$event)" ></div>		
							<div class="commodity-change-1 c-add1">+1</div>							
						</div>
					</li>
					<li class="all-li-pay"></li>
<!--					<li class="all-li-pay">总积分:<span>&nbsp;{{shopOrder.point}}</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;总金额:<span>¥&nbsp;{{shopOrder.money}}</span></li>-->
				</div>
				<!--<div class="order-details">
					<div class="o-details-n"><span>订单详情</span><img class="orders-hide-img" src="../../images/shop_down_arrow@2x.png"/></div>
					<div class="o-details-c">
						<div class="o-details-pice">总计:<span>  {{shopOrder.money}}</span> 元</div>
						<div class="o-details-pice">积分:<span>  {{shopOrder.point}}</span> 积分</div><span class="orders-hide-img"></span>
					</div>
				</div>-->
				<div class="pice-mode">
					<div class="pice-mode-n o-details-n"><span class="pay-tips"></span><span class="pay-tips-name">支付方式</span></div>
					<div class="pice-mode-choice"><span v-for="(item,index) in payType" v-bind:class="{active:item.pay_id==shopOrder.pay_type}" v-on:mousedown="changePay(item.pay_id,$event)" v-text="item.pay_name"></span></div>
				</div>
				<div class="pice-mode order-mode" style="display: none;">
					<div class="pice-mode-n o-details-n"><span>订单类型</span><img class="orders-hide-img" src="../../images/shop_down_arrow@2x.png"/></div>
					<div class="pice-mode-choice"><span class="active">店内消费</span><span>外送业务</span><span>店内自取</span></div>
				</div>
				<div class="order-information">
					<div class="pice-mode-n o-details-n"><span class="pay-tips"></span><span class="pay-tips-name">订单备注</span></div>
					<div class="o-room-infor"><span>店内桌号<em>*</em></span><input type="tel" placeholder="在此输入桌号" maxlength="8" v-model="shopOrder.room_id" /></div>
					<div class="o-room-infor"><span>配送地址<em>*</em></span><input type="tel" placeholder="如需配送请填写" maxlength="80" v-model="shopOrder.address" /></div>
					<div class="o-room-infor"><span>电话<em>*</em></span><input type="tel" placeholder="在此输入电话" maxlength="11" v-model="shopOrder.phone" /></div>
					<div class="o-room-infor"><span>称呼</span><input type="text" placeholder="在此输入称呼" maxlength="20" v-model="shopOrder.name" /></div>
					<div class="o-room-beizhu"><span>备注说明</span><div><textarea name="" rows="" cols="" placeholder="例如:少辣,多辣..." v-model="shopOrder.remark"  style="padding-left: 0.7rem;"></textarea></div></div>
				</div>
			</article>
			<!--<div style="position: relative;">--> 
			<div class="pice-pay" id="pice-pay">
				<div class="pice-order-all">总计：<span>{{shopDown.comNum}}</span>件</div>
				<div class="pice-pay-all">合计：<span>{{shopOrder.money}}</span>元</div>
				<div class="pice-pay-s" v-on:mousedown="orderNew()" >确认下单</div>
			</div>	
			<!--</div>-->
		</section>
		<div id="no_shopping" class="shoping-no" style="display: none;height:calc(100vh);background-color:#fff;">
			<div class="content">
				<ul class="">
					<li>
						<img src="../../images/restaurant/none_product_img@2x.png" alt="" />
						<p>您的购物车是空空的，赶紧行动吧！您可以</p>
					</li>
				</ul>
			</div>
			<div class="fabu"><router-link :to="{name:'lifeHome'}" tag="span" class="btn">点击选购</router-link></div>
		</div>
	</div>
</template>
<script type="text/javascript">
    require("./shoppingHome.css");
    export default {
        data: function(){
        	return {
        		shopOrder:{
        			shopid:configuration.global.shopid,
        			order:[],   //订单数组
        			token:sessionStorage.getItem("token")||"",
        			type:0,     //订单类型(0:店内消费，1：外送业务，2：店内自取)
        			pay_type:3, //付款方式（0：货到付款，1：店内消费，2：余额支付，3：微信支付）
        			room_id:"",  //房间号
        			name:"",
        			phone:"",
        			remark:"",
        			address:"",
        			money:0,  //总价
        			point:0  //总积分
        		},
        		shopDown:{
        			comNum:0
        		},
        		payType:[]
        	}
        },    	
        mounted: function () {
			this.initShopping();
			this.getPayType();
			globalMethod.setHscroll("shopHomePage");
        },
        methods: {
            backPage:function() {
            	globalMethod.layerUtils.iAlert("正在开发，敬请期待...");
                if (window.history && window.history.pushState)window.history.back();
           	},
		    redCommodity:function(item,event){
				globalMethod.changeLocalStorage(false,item,this.initShopping,event.target.previousElementSibling);
		    },
		    addCommodity:function(item,event){
		    	//修改购物车状态
		    	globalMethod.changeLocalStorage(true,item,this.initShopping,event.target.nextElementSibling);
		    },
		    initShopping:function(){
	            var shopCart = JSON.parse(localStorage.getItem("shopCart"))||[];
	            document.getElementById("has_shopping").style.display = shopCart.length === 0?"none":"block";
	            document.getElementById("no_shopping").style.display = shopCart.length === 0?"block":"none";
	            this.shopOrder.money = this.shopOrder.point = this.shopDown.comNum = 0;
	            if(shopCart.length === 0)return false;
	            this.shopOrder.order = shopCart;
	            for(var i = 0,shopLeng = shopCart.length;i < shopLeng;i++){
	            	this.shopOrder.money += Number(shopCart[i].price)*shopCart[i].num;
	            	this.shopOrder.point += Number(shopCart[i].point)*shopCart[i].num;
	            	this.shopDown.comNum += shopCart[i].num;
	            }	
		    },
		    goShop:function(){
		    	this.$router.push({name:"lifeHome"});
		    },
		    changePay:function(index,event){
		    	this.shopOrder.pay_type = index;
		    	$(event.target).addClass("active").siblings().removeClass("active");
		    },
		    orderNew:function(){
		    	if(this.checkShop()){
			        this.$http.post(configuration.global.serverPath + "/api/order/order",this.shopOrder,{headers: {'Content-Type': 'application/x-www-form-urlencoded'},emulateJSON:true}).then(function (response) {
			         	var results = response.data;
			         	if(results.code === 200){
			         		sessionStorage.setItem("payOrder",JSON.stringify({order:results.data.order,money:this.shopOrder.money}));
					      	sessionStorage.setItem("shopCart",localStorage.getItem("shopCart"));
					      	document.getElementById("choiceCommodityNum").style.display = "none";
					      	localStorage.removeItem("shopCart");			         		
			         		this.$router.push({name:this.shopOrder.pay_type == 3&&"orderPay"||"orderSuccess"});
			         	}else{
			         		globalMethod.layerUtils.iAlert(results.message||"请求服务器失败");
			         	}
			        }, function (response) {
			        	globalMethod.layerUtils.iAlert("连接服务器失败，请联系管理员");
			        });			    		
		    	}
		    },
		    checkShop:function(){
		    	if(this.shopDown.comNum === 0){
		    		globalMethod.layerUtils.iAlert("当前无下单商品");
		    		return false;
		    	}
		    	if(this.shopOrder.room_id.length === 0 && this.shopOrder.address.length === 0){
		    		globalMethod.layerUtils.iAlert("请输入桌号或配送地址");
		    		return false;
		    	}
		    	if(this.shopOrder.address.length !== 0 && this.shopOrder.phone.length === 0){
		    		globalMethod.layerUtils.iAlert("如需配送请填写手机号");
		    		return false;
		    	}
		    	if(this.shopOrder.phone.replace(/\s/g,"").length !== 0 && !/^(13|14|15|17|18)[0-9]{9}$/.test(this.shopOrder.phone)){
		    		globalMethod.layerUtils.iAlert("手机号码格式不正确");
		    		return false;
		    	}
		    	return true;
		    },
		    getPayType:function(){
		        this.$http.post(configuration.global.serverPath + "/api/order/getPayType",this.shopOrder,{headers: {'Content-Type': 'application/x-www-form-urlencoded'},emulateJSON:true}).then(function (response) {
		         	var results = response.data;
		         	if(results.code === 200){
		         		this.payType = results.data;
		         	}else{
		         		globalMethod.layerUtils.iAlert(results.message||"请求服务器失败");
		         	}
		        }, function (response) {
		        	globalMethod.layerUtils.iAlert("连接服务器失败，请联系管理员");
		        });			    	
		    }
        }
    }
</script>
