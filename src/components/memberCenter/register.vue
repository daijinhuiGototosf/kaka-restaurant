<template>
	<div id="register" style="position: relative;height:calc(100vh - 3.26rem);overflow-x: hidden;overflow-y: auto;">
		<section class="main fixed">
			<article class="content">
				<div class="login-head">
					<img src="../../images/restaurant/login_in_head_img@2x.png"/>
				</div>				
				<div class="pass-input pass-name">
					<span>用户名<em>*</em></span><input type="text" name="userName" id="userName" v-model="regParam.username" placeholder="请输入您的用户名" maxlength="20" />
				</div>
				<div class="pass-input">
					<span>密码<em>*</em></span><input type="password" name="pwd" v-model="regParam.password" placeholder="请设置登录密码" maxlength="20" />
				</div>
				<div class="pass-input r-passwd">
					<span>重输密码<em>*</em></span><input type="password" name="pwdr" id="pwdr" v-on:blur="checkPwd()" placeholder="请再一次输入密码" maxlength="20" />
					<p id="checkPwdS">*两次密码输入的不一致</p>
				</div>
				<!--<div class="pass-input">
					<span>邮箱</span><input type="email" name="email" v-model="regParam.email" placeholder="请输入邮箱" maxlength="30" />
				</div>
				<div class="pass-input">
					<span>姓名</span><input type="text" name="nickname" v-model="regParam.nickname" placeholder="请输入姓名" maxlength="20"/>
				</div>
				<div class="pass-input">
					<span>电话</span><input type="tel" name="phone" v-model="regParam.phone" placeholder="请输入电话" maxlength="11"/>
				</div>
				<div class="pass-input">
					<span>地址</span><input type="text" name="address" v-model="regParam.address" placeholder="请输入地址" maxlength="50"/>
				</div>
				<div class="pass-input">
					<span>QQ号</span><input type="tel" name="qq" v-model="regParam.qq" placeholder="请输入QQ号" maxlength="13"/>
				</div>-->
				<div class="register-btn">
					<div class="register-go" v-on:mousedown="goRegister()" >注册</div>
				</div>
			</article>		
		</section>
	</div>	
</template>
<script type="text/javascript">
    export default {
        data: function(){
        	return {
        		regParam:{
        			shopid:configuration.global.shopid,
        			username:"",
        			password:"",
        			email:"",
        			nickname:"",
        			phone:"",
        			address:"",
        			qq:""
        		},
        		isShow:false
        	}
        },    	
        mounted: function () {
        	globalMethod.setHscroll("register");
        },
        methods: {
        	isEnNumeric:function(strValue){
        		// 判断是否为英文、数字、字母或_
				var pattern = /^[_0-9a-zA-Z]+$/;  
				return pattern.test(strValue);        		
        	},
        	checkPwd:function(){
        		document.getElementById("checkPwdS").style.display = this.regParam.password === document.getElementById("pwdr").value?"none":"block";
        		return this.regParam.password === document.getElementById("pwdr").value;
        	},
        	goRegister:function(){
        		if(this.checkReg()){
			        this.$http.post(configuration.global.serverPath + "/api/login/register",this.regParam,{headers: {'Content-Type': 'application/x-www-form-urlencoded'},emulateJSON:true}).then(function (response) {
			         	var results = response.data;
		                if(results.code === 200){
		                	sessionStorage.setItem("token",results.data.token);
		                	globalMethod.layerUtils.iAlert("注册成功",function(){
		                		this.$router.push({name:sessionStorage.getItem("toLoginPageCode")});
		                	}.bind(this));
		                }else if(results.code === -1){
		                    globalMethod.layerUtils.iAlert(results.data);
		                }else{
		                	globalMethod.layerUtils.iAlert(results.data);
		                }
			        },function (response) {
			        	alert(JSON.stringify(response.data));
			        });         			
        		}
        	},
        	checkReg:function(){
        		if(!this.regParam.username || !this.isEnNumeric(this.regParam.username)){
        			globalMethod.layerUtils.iAlert("用户名输入不正确!");
        			return false;
        		}
        		if(!this.regParam.password || !this.regParam.password.replace(/\s/g,"")){
        			globalMethod.layerUtils.iAlert("请设置密码!");
        			return false;
        		}
        		if(!this.checkPwd()){
        			return false;
        		}
        		return true;        		
        	}
        }
    }
</script>
<style>
	#register{
		background-color: #fff;
	}
	.isShow{
		display: none;
	}
	.pass-input{
		width: 84%;
		height: auto;
		border-bottom: 1px solid #E5E5E5;
		margin: 0 8%;
		padding-top: 1.5rem;
	}
	.pass-input > span{
	    font-size: 1.2rem;
	    color: #333;
	    display: inline-block;
	    width: 26%;
	    text-align: right;	
	    height: 2.5rem;	
	}
	.pass-input > input{
		margin-left: 6%;
		width: 68%;
		height: 2rem;
		line-height: 2rem;
		font-size: 1.1rem;
		color: #999;
		border: 0;
	}
	.pass-input em{
		color: red;
	}
	.r-passwd{
	
	}
	.r-passwd > span{
	}
	.r-passwd > input{
		/*margin-top: 1.5rem;*/
	}
	.r-passwd p{
		display: none;
		color: red;
	}
	.register-btn{
		width: 100%;
		height: 8rem;
		background-color: #fff;
		padding-top: 2rem;
	}
	.register-go{
		height: 2.6666666666666665rem;
		line-height: 2.6666666666666665rem;
		background-color: #ff5959;
		text-align: center;
		font-size: 1.2rem;
		color: #fff;
		border-radius: 40px;
		margin: 0 8%;
	}
	.login-head{
		position: relative;
		width: 100%;
		height: 8.233333333333333rem;
		background: linear-gradient(to right top,#ff5959,#ff9146);
	}
	.login-head img{
		width: 5.466666666666667rem;
		height: 5.466666666666667rem;
		position: absolute;
		display: inline-block;
		bottom: -1rem;
		left: 50%;
		transform: translateX(-50%);
	}
	.pass-name{
		margin-top: 1rem;
	}	
</style>