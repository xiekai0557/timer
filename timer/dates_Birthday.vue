<template>
	<div @click.stop class="dates">
		<div class="date" @click="show(1)">
			<input  type="text" disabled :value="dater" />
			<i class="iconfont icon-jiantou2"></i>
		</div>
		
		<transition enter-active-class="fadeIn" leave-active-class="fadeOut">
			<div id="aaabox" class="sele animated" v-show='fadeIn' >
				<div class="sele_top">
					<span @click="timeA(1,yaerNum)"  class="iconfont icon-shuangjiantou"></span>
					<span v-show="dayShow || timerShow" @click="timeA(3,0)" class="iconfont icon-zuojiantou-"></span>
					<span @click='cliYea()' class="year">{{year}}年</span>
					<span v-show="dayShow || timerShow" @click='cliMonth()' class="month">{{month | ling}}月</span>
					<span v-show="dayShow || timerShow" @click="timeA(4,0)" class="iconfont icon-youjiantou-"></span>
					<span @click="timeA(2,yaerNum)" class="iconfont icon-next_rf"></span>
				</div>
				
				<div class="week">
					<transition enter-active-class="zoomInLeft" leave-active-class="zoomOutRight">
						<div class="day animated" v-show="dayShow">
							<ul class="week_num">
								<li>日</li>
								<li>一</li>
								<li>二</li>
								<li>三</li>
								<li>四</li>
								<li>五</li>
								<li>六</li>
							</ul>
							<ul class="week_li">
								<li :class="{LastDays:'LastDays'==wee.clas,newActive:'active'==wee.newActive,active:'active1'==wee.clasActive,NoClass:'noclass'==wee.NoClass}" v-for="(wee,index) in week" @click="wee.click &&clasActive(index,wee.month)" :value="wee.month" >
									{{wee.li}}
								</li>
							</ul>
						</div>
					</transition>
					
					<transition enter-active-class="zoomInLeft" leave-active-class="zoomOutRight">
						<div class="year animated" v-show="yearShow" >
							<ul>
								<li v-for="ye in yea" :class="{active:year==ye}"><span @click=" yearLi(ye)">{{ye}}</span></li>
							</ul>
						</div>
					</transition>
					
					<transition enter-active-class="zoomInLeft" leave-active-class="zoomOutRight">
						<div class="month animated" v-show="monthShow">
							<ul>
								<li v-for="mont in mon" :class="{active:mont==month}"><span @click="monthLi(mont)">{{mont}}</span></li>
							</ul>
						</div>
					</transition>
					
					
				</div>
				<div class="determine">
					<button @click="Determine()">确定</button>
				</div>
			</div>
		</transition>
	</div>
</template>

<script>
	

	export default{
		data(){
			return {
				week:[],
				yea:[],
				mon:[1,2,3,4,5,6,7,8,9,10,11,12],
				weekTimer:[],
				weekBranch:[],
				year:'2018',
				month:'07',
				day:'01',
				dater:'2018-07-01',
				fadeIn:0,
				dayShow:1,
				yearShow:0,
				monthShow:0,
				timerShow:1,
				yaerNum:1,
				maopao:0,
			}
		},
		props:['BirTimer'],
		methods:{
			//点击时间选择输入框
			show(fadeIn){
				this.year = this.dater.split('-')[0]
				this.month = parseInt( this.dater.split('-')[1] )
				this.weekLi();
				this.fadeIn = fadeIn
			},
			//点击确定
			Determine(){
				this.fadeIn = !this.fadeIn
				this.day = this.day<10?'0'+parseInt(this.day):parseInt(this.day)
				this.dater = this.year+'-'+( this.month<10?'0'+parseInt(this.month):parseInt(this.month) )+'-'+this.day
				this.yearShow = 0;
				this.dayShow = 1;
				this.monthShow = 0;
				this.timerShow = 1;
				this.BirTimer.timer = this.dater
			},
			//点击年
			cliYea(){
				this.yearShow = 1;
				this.dayShow = 0;
				this.monthShow = 0;
				this.yaerNum = 10;
				this.yea = [];
				for( var i=0;i<10;i++ ){
					this.yea.push( (this.year+'').slice(0,3)+i )
				}
			},
			//点击年的li
			yearLi(year){
				this.year = year
				this.yearShow = 0;
				this.dayShow = 0;
				this.monthShow = 1;
				this.yaerNum = 1;
			},
			//点击月
			cliMonth(){
				this.yearShow = 0;
				this.dayShow = 0;
				this.monthShow = 1;
				this.yaerNum = 1;
			},
			//点击月的li
			monthLi(month){
				this.month = month
				this.yearShow = 0;
				this.dayShow = 1;
				this.monthShow = 0;
				this.yaerNum = 1
				this.weekLi()
				
			},
			//点击上一年、下一年、上一月、下一月
			timeA(n,num){
				
				//上一年
				if( n==1 ){
					this.year -=num
					if( this.year <= 1700 ){
						this.year =1700
					}
					if( num==10 ){
						this.cliYea()
					}
				}
				//下一年
				if( n==2 ){
					this.year =  Number(this.year)+num 
					
					if( this.year >= 3000 ){
						this.year =3000
					}
					if( num==10 ){
						this.cliYea()
					}
				}
				//上一月
				if( n==3 ){
					this.month--
					if( this.month<=0 ){
						this.month = 12
						this.year--;
						if( this.year < 1700 ){
							this.year =1700
							this.month =1
						}
					}
				}
				//上一月
				if( n==4 ){
					this.month++
					if( this.month>12 ){
						this.month = 1
						this.year++
						if( this.year > 3000 ){
							this.year =3000
							this.month =12
						}
					}
				}
				this.weekLi();
				
			},
			//点击LI
			clasActive(index,timer){
				
				timer = timer.split('-')
				
				this.year = timer[0];
				this.month = timer[1]<10?'0'+parseInt( timer[1] ):parseInt(timer[1]);
				this.day = timer[2]<10?'0'+parseInt(timer[2]):timer[2];
				this.dater = this.year+'-'+this.month+'-'+this.day;
				this.weekLi();
				this.BirTimer.timer = this.dater
			},
			//加载所有的li
			weekLi(){
				this.week = []
				this.weekTimer = []
				this.weekBranch = []
				var Timer = ''
				var Branch = ''
				for( var i=0;i<24;i++ ){
					Timer = i<10?'0'+i:i
					this.weekTimer.push({li:Timer,weekTimeClass:''} )
				}
				for( var i=0;i<60;i++ ){
					Branch = i<10?'0'+i:i
					this.weekBranch.push({li:Branch,weekBranchClass:''} )
				}
				
				//获取当前
				var myDate = new Date();//获取系统当前时间
				var myYear = myDate.getFullYear(); //获取完整的年份(4位,1970-????)
				var myMonth= myDate.getMonth()+1; //获取当前月份(0-11,0代表1月)
				var myDate = myDate.getDate(); //获取当前日(1-31)

				var dt = new Date(this.year,this.month-1, 1).getDay();//本月1号星期几
				//构造当前日期对象
				var date = new Date();
				//获取年份
				var year = date.getFullYear();
				var NewDay = date.getDate();
				//获取当前月份
				var mouth = this.month-1 + 1;
				var nowMonth = date.getMonth()+1;
				//定义当月的天数；
				var LastDays ;
				var days ;
				var NextDays;
				//月份为二月时，根据闰年还是非闰年判断天数
				if(mouth-1 == 2){
				    LastDays= this.year % 4 == 0 ? 29 : 28;   
				    
				}else if(mouth-1 == 1 || mouth-1 == 3 || mouth-1 == 5 || mouth-1 == 7 || mouth-1 == 8 || mouth-1 == 10 || mouth-1 == 12){
			        //月份为：1,3,5,7,8,10,12 时，为大月.则天数为31；
			        if( mouth-1<=0 ){
			        	LastDays= 30;
			        }
			        LastDays= 31;
				}else{
			        //其他月份，天数为：30.
			        LastDays= 30;
				}
				
				//当月份为二月时，根据闰年还是非闰年判断天数
				if(mouth == 2){
				    days= this.year % 4 == 0 ? 29 : 28; 
				}else if(mouth == 1 || mouth == 3 || mouth == 5 || mouth == 7 || mouth == 8 || mouth == 10 || mouth == 12){
			        //月份为：1,3,5,7,8,10,12 时，为大月.则天数为31；
			        days= 31;
				}else{
			        //其他月份，天数为：30.
			        days= 30;
				}
				
			    //输出天数
				days = days+1
				LastDays = LastDays+1
				var LastDaysDt = LastDays-dt
				
				var Lastyear = this.year;
				var LastMonth = this.month-1;
				//上个月
				if( LastDays==LastDaysDt ){
					LastDaysDt = LastDaysDt-7
					
				}
				if( LastMonth<=0 ){
					LastMonth = 12
					Lastyear--
				}
				
				for( var i=LastDaysDt;i<LastDays;i++ ){
					this.week.push({clas:'LastDays',li:i,month:Lastyear+'-'+LastMonth+'-'+i})
				}
				
				
				//本月
				var daterY = this.dater.split(' ')[0].split('-');
				
				for( var i=1;i<days;i++ ){
					if( i == NewDay && nowMonth==mouth && myYear==this.year ){
						this.week.push({newActive:'active',li:i,month:this.year+'-'+this.month+'-'+i})
					}else if( daterY[0]==this.year && daterY[1]==this.month && daterY[2]== i){
						this.week.push({clasActive:'active1',li:i,month:this.year+'-'+this.month+'-'+i})
					}else{
						this.week.push({li:i,month:this.year+'-'+this.month+'-'+i})
					}
				}
				
				//下月
				var NextDays = 42-this.week.length+1
				var Nextyear = this.year;
				var NextMonth = Number( this.month)+1;
				if(  NextMonth>12 ){
					NextMonth = 1
					Nextyear++
				}
				for( var i=1;i<NextDays;i++ ){
					this.week.push({clas:'LastDays',li:i,month:Nextyear+'-'+NextMonth+'-'+i})
				}
				

				//无法点击的
				var numI = LastDays-LastDaysDt+1
				var numII = 0;
				
				//给所有LI加上可以点击的事件
				for(var i=0;i<this.week.length;i++){
					this.week[i].click = true
				}
				
				//如果现在的年小于设置的年 
				if( myYear<this.year  ){
					for( var i=0;i<this.week.length;i++ ){
						
						this.week[i].NoClass= 'noclass';
						this.week[i].click = false;
					}
					return 
				}
				
				//如果现在的年小于设置的年 并且 现在的月 小于设置的月
				if( myYear==this.year && myMonth<this.month ){
					for( var i=0;i<this.week.length;i++ ){
						
						this.week[i].NoClass= 'noclass';
						this.week[i].click = false;
					}
					return 
				}
				
				//如果现在的年小于设置的年   并且  现在月小于设置的月
				if( myYear<this.year && myMonth<this.month ){
					for( var i=0;i<this.week.length;i++ ){
						
						this.week[i].NoClass= 'noclass';
						this.week[i].click = false;
					}
				}
				
				if( myYear==this.year && myMonth==this.month  ){
					for ( var i=numI;i<this.week.length;i++ ) {
						if( i>myDate+numI-2 ){
							this.week[i].NoClass= 'noclass';
							this.week[i].click = false;
						}
					}
				}	
			}
			
		},
 		watch:{
 			BirTimer:{
 				handler(val){
 					this.Birthday = val;
 					this.dater = this.Birthday.timer
 				},
 				deep: true
 			}
 		},
		mounted(){
			this.weekLi()
			this.dater = this.BirTimer.timer
			let _this = this
			$(document).click(function(e){
				
					_this.fadeIn = false
					_this.yearShow = 0;
					_this.dayShow = 1;
					_this.monthShow = 0;
					_this.timerShow = 1;
			})
		},
		transitions:{
			zoom:{
				enterClass:'zoomIn',
				leaveClass:'zoomOut'
			}
		}
	}
	
</script>

<style>
	.dates{position:relative;width:170px;}
	.dates .date{width:170px;height:30px;border:1px solid #dcdcdc;position:relative;cursor:pointer;transition: .3s; -moz-transition: .3s; /* Firefox 4 */ -webkit-transition: .3s; /* Safari 和 Chrome */ -o-transition:.3s; /* Opera */;border-radius:3px;overflow:hidden;}
	.dates .date:hover{border:1px solid #0099FF;}
	.dates .date input{width:100%;height:100%;border:none;padding:0;position:absolute;left:0px;top:0px;background:#fff;cursor:pointer;text-indent:10px;font-size:14px;color:#666;}
	.dates .date i{width:30px;height:30px;position:absolute;right:0px;top:0px;background:#eff2f7;border-left:1px solid #dcdcdc;font-size:12px;color:#999;text-align:center;line-height:35px;}
	
	.dates .sele{width:215px;height:290px;background:#fff;position:absolute;left:0px;top:40px;box-shadow:0 1px 6px rgba(0,0,0,.2);z-index:3;}
	.dates .sele .sele_top{width:100%;height:32px;line-height:32px;border-bottom:1px solid #dcdcdc;text-align:center;}
	.dates .sele .sele_top .iconfont{color:#bbb;cursor:pointer;display:block;float:left;font-size:13px;transition: .3s; -moz-transition: .3s; /* Firefox 4 */ -webkit-transition: .3s; /* Safari 和 Chrome */ -o-transition:.3s; /* Opera */;border-radius:3px;}
	
	.icon-shuangjiantou{position:absolute;left:12px;top:0px;}
	.icon-zuojiantou-{position:absolute;left:30px;top:0px;}
	.year{color:#09f;cursor:pointer;font-weight:500;}
	.month{color:#09f;cursor:pointer;font-weight:500;}
	.icon-youjiantou-{position:absolute;right:30px;top:0px;}
	.icon-next_rf{position:absolute;right:12px;top:0px;}
	.dates .sele .sele_top .iconfont:hover{color:#09f;}
	
	.week{height:191px;padding:12px;border-bottom:1px solid #dcdcdc;overflow:hidden;position:relative;}
	.week .day {position:absolute;left:12px;top:12px;}
	.week .day .week_num li{color:#bbbec4;}
	.week .day li{float:left;width:27.2px;text-align:center;height:27px !important;line-height:27px !important;}
	.week .day .week_li{cursor:pointer;}
	.week .day .week_li li{background:#fff;border-radius:3px;transition: .3s; -moz-transition: .3s; /* Firefox 4 */ -webkit-transition: .3s; /* Safari 和 Chrome */ -o-transition:.3s; /* Opera */;}
	.week .day .week_li li:hover{background:#e1f0fe;}
	
	.week .day .week_li li.LastDays{color:#bbbec4;}
	.week .day .week_li li.LastDays:hover{background:#fff;}
	
	.week .day .week_li li.active{color:#fff;background:#0099FF;}
	.week .day .week_li li.newActive{position:relative;}
	.week .day .week_li li.newActive::after{content:'';width:6px;height:6px;position:absolute;right:1px;top:3px;background:#0099FF;border-radius:100%;}
	.week .day .week_li li.active.newActive::after{background:#fff;}
	.week .day .week_li li.LastDays.active{color:#BBBEC4;background:#fff;}
	
	.week .day .week_li li.NoClass{cursor:not-allowed;background:#f7f7f7;border-radius:0px;color:#BBBEC4;}
	.week .day .week_li li.NoClass:hover{background:#f7f7f7;}
	.week .year{width:191px;height:191px;margin:0px;color:#666;position:absolute;left:12px;top:12px;}
	.week .year li{float:left;width:33%;text-align:center;height:50px;}
	.week .year li span{padding:3px 5px;border-radius:3px;}
	.week .year li.active span{background:#09f;color:#fff;transition: .3s; -moz-transition: .3s; /* Firefox 4 */ -webkit-transition: .3s; /* Safari 和 Chrome */ -o-transition:.3s; /* Opera */;}
	.week .year li span:hover{background:#E1f0fe;}
	.week .year li.active span:hover{background:#09f;}
	
	.week .month{width:191px;height:191px;color:#666;position:absolute;left:12px;top:12px;}
	.week .month li {width:33%;text-align:center;float:left;height:50px;}
	.week .month li span{padding:5px 16px;border-radius:3px;}
	.week .month li.active span{background:#09f;color:#fff;transition: .3s; -moz-transition: .3s; /* Firefox 4 */ -webkit-transition: .3s; /* Safari 和 Chrome */ -o-transition:.3s; /* Opera */;}
	.week .month li span:hover{background:#E1f0fe;}
	.week .month li.active span:hover{background:#09f;}
	
	.week .week_timer{position:absolute;left:0px;top:0px;width:215px;height:215px;}
	.week .week_time{border-right:1px solid #dcdcdc;}
	.week .week_time,.week .week_branch{float:left;width:107px;overflow:auto;height:215px;text-align:center;}
	.week .week_time li,.week .week_branch li{height:32px !important;line-height:32px !important;cursor:pointer;background:#fff;transition: .3s; -moz-transition: .3s; /* Firefox 4 */ -webkit-transition: .3s; /* Safari 和 Chrome */ -o-transition:.3s; /* Opera */;}
	.week .week_time li:hover,.week .week_branch li:hover{background:#f3f3f3;}
	.week .week_time li.active,.week .week_branch li.active{background:#f3f3f3;color:#09f;}
	
	.determine {padding:0px 12px;line-height:40px;}
	.determine span{font-size:14px;cursor:pointer;color:#09f;transition: .3s; -moz-transition: .3s; /* Firefox 4 */ -webkit-transition: .3s; /* Safari 和 Chrome */ -o-transition:.3s; /* Opera */;}
	.determine span:hover{color:#57a3f3;}
	.determine button{float:right;width:40px;height:24px;font-size:12px;border-radius:3px;border:none;color:#fff;background:#09f;margin-top:8px;cursor:pointer;transition: .3s; -moz-transition: .3s; /* Firefox 4 */ -webkit-transition: .3s; /* Safari 和 Chrome */ -o-transition:.3s; /* Opera */;}
	.determine button:hover{background:#57a3f3;}
	
	.turnsAdd .content ul li .date input:focus, .content ul li .date input:hover{border:none;}
	
	 .week .week_time::-webkit-scrollbar,.week .week_branch::-webkit-scrollbar {/*滚动条整体样式*/
            width: 10px;     /*高宽分别对应横竖滚动条的尺寸*/
            /*height: 4px;*/
        }
        .week .week_time::-webkit-scrollbar-thumb,.week .week_branch::-webkit-scrollbar-thumb {/*滚动条里面小方块*/
            border-radius: 5px;
            -webkit-box-shadow: inset 0 0 5px rgba(0,0,0,0.2);
            background: #bec0c4;;
            transition: .3s; -moz-transition: .3s; /* Firefox 4 */ -webkit-transition: .3s; /* Safari 和 Chrome */ -o-transition:.3s; /* Opera */
        }
       .week .week_time::-webkit-scrollbar-track,.week .week_branch::-webkit-scrollbar-track {/*滚动条里面轨道*/
            /*-webkit-box-shadow: inset 0 0 5px rgba(0,0,0,0.2);*/
            border-radius: 0;
            background:none;
        }
       .week .week_time::-webkit-scrollbar-thumb:hover,.week .week_branch::-webkit-scrollbar-thumb:hover {/*滚动条里面轨道*/
            border-radius: 5px;
            -webkit-box-shadow: inset 0 0 5px rgba(0,0,0,0.2);
            background:#a5a8ab;
        }
</style>

