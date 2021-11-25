<template>
	<view class="content">
		<view class="count">
			<view class="output">
				{{result}}
			</view>
			<view class="button">
				<button v-for="(item,index) in UI" @click="get_value(item)">
					{{item}}
				</button>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				UI: ['%',"CE",'C',"BS",'7','8','9','÷','4','5','6',
					'×','1','2','3','-','.','0','=','+'],
				result: '',
				number: 0,
				temp: 0,
				flag: "num",
				operator: '0',
				dot: 0,
				fac: 0.1,
			}
		},
		onLoad() {
			
		},
		methods: {

			get_value:function value(e) {
				if(e>='0'&&e<='9') {
					if(this.operator==='=') {
						if(!this.dot) {
							this.result='';
							this.number=0;
						}
						this.operator='0';
					}
					if(this.dot) {
						this.temp += this.fac*Number(e);
						this.fac/=10;
					}
					else 
						this.temp = this.temp*10 + Number(e);
					this.result += e;
					this.flag="num";
					this.dot=0;
				}
				switch(e) {
					case '=':
						this.number=eval(String(this.number+this.operator+this.temp));
						this.result=this.number;
						this.flag=''; this.operator='=';
						this.fac=0.1; this.dot= 0;
						this.temp=0; break;
					case 'C':
						this.fac=0.1; this.dot= 0;
						this.number=0; this.operator='0'; this.flag="num";
						this.result=''; this.temp=0; break;
					case "CE":
						this.temp=0;
						this.result=this.number+this.operator;
						break;
					case "BS":
						this.temp -= this.temp%10;
						this.temp/=10;
						this.result=this.number+this.operator+this.temp;
						break;
					case '.':
						this.fac=0.1; this.dot=1;
						this.result+='.';
						break;
					case '%':
						this.temp*=0.01;
						if(this.operator==='0') this.result=this.temp;
						else this.result=this.number+this.operator+this.temp;
						break;
					case '+':
					case '-':
					case '×':
					case '÷':
						if(this.operator==='0') this.operator='+';
						if(e==='×') e='*'; if(e==='÷') e='/';
						if(this.flag==="num") {
							this.number=eval(String(this.number+this.operator+this.temp));
							this.temp=0;
						}
						this.operator=e;
						if(e==='*') e='×'; if(e==='/') e='÷';
						this.result=this.number+e;
						this.flag="ope"; this.dot=0;
				}
			},
		}
	}
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}
	.count .output {
		font-size: 90rpx;
		background-color: #F1F1F1;
		text-align: right;
		margin: auto;
		width: 700rpx;
		height: 160rpx;
		overflow: scroll;
	}
	.count .button{
		display: flex;
		flex-direction: row;
		flex-wrap: wrap;
		margin-left: 25rpx;
	}
	.count button {
		margin: 0;
		text-align: center;
		font-size: 50rpx;
		height: 120rpx;
		width: 175rpx;
	}
</style>
