<template>
  <div>
    <!--页头-->
    <div id="header">
      
      <h2 style="display:inline-block;margin-left:40px">入住</h2>
    </div>
    <!--进度条-->
    <el-progress
      :show-text="false"
      :stroke-width="20"
      :percentage="90"
    ></el-progress>

    <!--主体部分-->
    <div id="mymain">
      <!--主功能区-->
      <div id="workspace">
    

          <!--入住时长-->
        <div id="interval">
            <h1>房客可以住多久？</h1>
            <small style="margin-top:10px;display:block;color:#778899">较短的住宿天数可以帮助您获得更多的预订，但您需要更频繁地清洁整理房源。</small>

            <div id="input-number" style="margin-top:4%">
                <i style="display:inline-block;font-size:1.2em;color:#196ebe" class="iconfont icon-calendar__eas"></i>
                <small style="display:inline-block;margin-left:10px;margin-right:20px">最少天数</small>
                <el-input-number size="small" v-model="minDay"  :min="1" label="最少天数"></el-input-number>
            </div>
            <div id="input-number" style="margin-top:4%">
                <i style="display:inline-block;font-size:1.2em;color:#196ebe" class="iconfont icon-calendar__eas"></i>
                <small style="display:inline-block;margin-left:10px;margin-right:20px">最多天数</small>
                <el-input-number size="small" v-model="maxDay" :min="minDay" label="最多天数" @change="examine"></el-input-number>
            </div>
        </div>

        <!--入住时间-->
        <div id="checkin">
            <h1>房客什么时候可以入住？</h1>
            <div id="startTime" >
              <div>
            <i style="display:inline-block;font-size:1em;color:#196ebe" class="iconfont icon-shijian1"></i>
            <small style="display:inline-block;margin-left:10px;margin-bottom:10px;">从:</small>
              </div>
                <el-time-select
                v-model="startTime"
                placeholder="起始时间"
                :picker-options="{
                start: '08:00',
                step: '01:00',
                end: '22:00'
                }">
                </el-time-select>
            </div>

            <div id="endTime" style="margin-top:4%">
                <div>
            <i style="display:inline-block;font-size:1em;color:#196ebe" class="iconfont icon-shijian1"></i>
            <small style="display:inline-block;margin-left:10px;margin-bottom:10px;">到:</small>
              </div>
                <el-time-select
                v-model="endTime"
                    placeholder="结束时间"
                    :picker-options="{
                    start: '09:00',
                    step: '01:00',
                    end: '23:00',
                    minTime: startTime
                    }">
                </el-time-select>
            </div>

        </div>
      </div>

      <!--页尾-->
      <div style="border-top:1px solid #000;" id="footer">
        <el-button type="text" style="margin-top:10px;color:#63aaf1;font-weight:bolder"  @click="backPage">返回</el-button>
        <el-button style="margin-top:10px;color:white;float:right;display:inline-block;margin-right:10px;background-color:#63aaf1" @click="nextPage">下一个</el-button>
      </div>
    </div>

  </div>
</template>

<style scoped>
@import "//at.alicdn.com/t/font_2666163_1eek1wz6kww.css";
#header {
  text-align: left;
  height: 60px;
  background-color: white;
}

#workspace {
  display: absolute;
  padding: 20px 10px 20px 150px;
  background-color: white;
  width: 600px;
  height: 480px;
  text-align: left;
       overflow:auto;

}
div::-webkit-scrollbar {
  width: 0;
}
#footer {
  display: absolute;
  padding: 0 10px 0 150px;
  background-color: white;
  width: 600px;
  height: 80px;
  text-align: left;
}

#mymain {
  height: 600px;
  right: 0 !important;
  background-color: #b2d2f1;
}
</style>

<script>
export default {
    data:function(){
        return {
        startTime:'',
        endTime:'',
        minDay:1,
        maxDay:1,

        show:false,
        show2:false,
        }
    },

    

    mounted(){
      // 查看用户是否有权限进入该页面
      let userPermissions=localStorage.getItem("userPermissions").split(",")
      for(let i=0;i<userPermissions.length;++i){
        if(userPermissions[i]=="upgrade"){
            this.$message({
            message: "请您首先成为房东",
            type: 'warning'
          });
          // 跳转到个人信息界面
          this.$router.push({
            path: "/userInfoPage"
          })
          return;
        }
    
      }
      if(localStorage.getItem('startTime')){
      try {
        console.log('从浏览器获取startTime');
        this.startTime = JSON.parse(localStorage.getItem('startTime'));
      } catch(e) {
        console.log(e);
        localStorage.removeItem('startTime');
      }
    }

    if(localStorage.getItem('endTime')){
      try {
        console.log('从浏览器获取endTime');
        this.endTime = JSON.parse(localStorage.getItem('endTime'));
      } catch(e) {
        console.log(e);
        localStorage.removeItem('endTime');
      }
    }

    if(localStorage.getItem('minDay')){
      try {
        console.log('从浏览器获取minDay');
        this.minDay = JSON.parse(localStorage.getItem('minDay'));
      } catch(e) {
        console.log(e);
        localStorage.removeItem('minDay');
      }
    }

    if(localStorage.getItem('maxDay')){
      try {
        console.log('从浏览器获取maxDay');
        this.maxDay = JSON.parse(localStorage.getItem('maxDay'));
      } catch(e) {
        console.log(e);
        localStorage.removeItem('maxDay');
      }
    }

    },
    beforeRouteLeave(to,from,next){
    console.log('当前路由跳转to:',to.path.substr(0,15));
          if(to.path.substr(0,15)!='/become-a-host/'){
          console.log('当前路由跳转to:',to);
          this.clearStorage();
  }
  next();

},

    methods:{
      clearStorage(){
          let paramList=['stayType','maxTenantNum','roomNum','bedNum','pubRestNum','pubBathNum','barrierFree',
          'longitude','latitude','stayName','stayChars','stayTags','startTime','endTime','minDay','maxDay','struPos','roomInfo','imgResults','stayAlter','stayId'];

          for(let i=0;i<paramList.length;i++){
            localStorage.removeItem(paramList[i]);
          }
          console.log('清除浏览器记录！');
        },
      
        examine:function(event){
            console.log(event);
           if(this.maxDay>=this.minDay){
                console.log('正常')
                this.show=false;
            }
        },

        nextPage:function(){
            if(this.maxDay<this.minDay){
                console.log('最长入住时间出错！');
                this.$message({
                  message:'最短天数不能大于最长天数!',
                  type:'error',
                  duration:1500,
                  center:true
                })
                return ;
            }
            if(this.startTime==''||this.endTime==''){
              console.log('信息填写不完整！');
              this.$message({
                message:'请先完成本页面信息填写！',
                type:'warning',
                duration:1500,
                center:true
              })
              return ;
            }
            else{
              this.show2=false;
              const parsed = JSON.stringify(this.startTime);
              localStorage.setItem('startTime', parsed);

              const parsed1 = JSON.stringify(this.endTime);
              localStorage.setItem('endTime', parsed1);

              const parsed2 = JSON.stringify(this.minDay);
              localStorage.setItem('minDay', parsed2);

              const parsed3 = JSON.stringify(this.maxDay);
              localStorage.setItem('maxDay', parsed3);

              this.$router.push('/become-a-host/commit');
            }
        },

        backPage:function(){
          this.$router.go(-1);
        }
    }
}
</script>

