<template>
	<div id="memberPoints" style="position: relative;">
		<section class="main fixed">
			<article class="content">
				<div class="bgstyle">
					<div class="vipcard"  v-bind:style="styleObject()">
						<div class="vipnumber" v-if="!!messInfo.image" ></div>
					</div>
				</div>
				<div class="vipcard2">
					<ul>
						<li class="bgstyle vipmm">
							<a v-on:mousedown="goChange()" href="javascript:void(0);"> 积分：<em v-text="messInfo.point||0"></em>分 点击此处可以查看您的积分明细</a>
						</li>
						<li class="bgstyle vippr">会员卡使用注意事项：<br>
							<p>1、会员凭此卡享受消费积分。</p>
							<p>2、积分可用于兑换本店礼品及冲抵消费之用。</p>
							<p>3、本卡仅限本人使用，不得转借他人。 </p>
							<p>4、使用本卡时需向服务员出示此卡。 </p>
						</li>
					</ul>
				</div>
			</article>
		</section>
	</div>
</template>
<script type="text/javascript">
    export default {
        data: function(){
        	return {
        		messInfo:{}
        	}
        },    	
        mounted: function () {
        	this.getPoints();
        },
        methods: {
		    getPoints:function(page){
		        this.$http.post(configuration.global.serverPath + "/api/Personal/getMemeber",{token:sessionStorage.getItem("token")},{headers: {'Content-Type': 'application/x-www-form-urlencoded'},emulateJSON:true}).then(function (response) {
		         	var results = response.data;
		         	if(results.code === 200){
		         		this.messInfo = results.data;
//		         		$("#memberPoints .vippr").html("会员卡使用注意事项：<br>"+this.messInfo.note);
		         	}else{
		         		globalMethod.layerUtils.iAlert(results.message||"请求服务器失败");
		         	}
		        }, function (response) {
		        	globalMethod.layerUtils.iAlert("连接服务器失败，请联系管理员");
		        });			    		
		    },
		    goChange:function(){
		    	this.$router.push({name:"pointsChange"});
		    },
		    styleObject:function(){
		    	var imgage = this.messInfo.image||'src/images/member_membership_card@2x.png';
		    	return 'background:url(' + imgage + ') no-repeat #fff center';
		    }
        }
    }
</script>
<style>
	.bgstyle{
	    padding-top:0.4rem;
	}
	.vipcard {
		background: url("../../images/member_membership_card@2x.png")center center no-repeat;
		width: 300px;
		height: 191px;
		background-size: 300px 191px !important;
		overflow: hidden;
		margin: 0 auto
	}
	.vipcard2{
	    margin:0 auto;
	    margin-top:0.4rem;
	
	}
	.vipcard2 .vipmm{
	    background:#fff;
	    padding-top:0;
	    padding-left:0.8rem;
	    padding-right:0.8rem;
	    height:2.6rem;
	    line-height:2.6rem;
	
	
	
	}
	.bgstyle >a{
	    display: block;
	    background: url(../../images/life_next_pages_arrow@2x.png) no-repeat  right;
	    background-size: 0.6rem 1rem;
	    margin-right: 0.3rem;
	    font-size:0.9333333333333333rem;
	    color:#333333;
	}
	.bgstyle>a>em{
	    color:orangered;
	}
	
	.vipcard2 .vippr{
	    height:8.233333333333333rem;
	    font-size:0.9333333333333333rem;
	    color:#333333;
	    background:#fff;
	    margin-top:0.26666666666666666rem;
	    padding-left:0.8rem;
	}
	.vipcard2 .vippr>p{
	    font-size:0.8rem;
	    color:#666666;
		margin-top:0.25rem;
	}
</style>
