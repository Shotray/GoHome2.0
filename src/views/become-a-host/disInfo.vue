<template>
  <div>
    <!--页头-->
    <div id="header">
      
      <h2 style="display:inline-block;margin-left:40px">描述</h2>
    </div>
    <!--进度条-->
    <el-progress
      :show-text="false"
      :stroke-width="20"
      :percentage="71"
    ></el-progress>

    <!--主体部分-->
    <div id="mymain">
      <!--主功能区-->
      <div id="workspace">
        

        <div id="name">
            <div>
            <h1>为您的房源起个名字</h1>
            <i style="display:inline-block;font-size:1.5em;color:#196ebe" class="iconfont icon-bianji7"></i>
            <small style="display:inline-block;margin-left:10px;">为房源起一个能突出其独特之处的标题，吸引更多房客。</small>

            <div style="margin-top:10px;">
            <el-input 
              maxlength="10"
              show-word-limit
              v-model="name" 
              style="width:95%" 
              placeholder="房源名称">
            </el-input>
            <small style="margin-top:10px;display:block;color:#778899">好的房源标题可以增加房客的点击率，房屋名称可包含地标、位置、附近交通、房屋特色等信息。</small>
            </div>
            </div>

            <div style="margin-top:8%">
            <h1>描述下您的房源</h1>
            <i style="display:inline-block;font-size:1.5em;color:#196ebe" class="iconfont icon-bianji3"></i>
            <small style="margin-left:10px;margin-top:10px;display:inline-block;">描述房源特色。</small>
            
            <div style="margin-top:10px;">
            <el-input 
                maxlength="100"
                show-word-limit
                type="textarea"
                :autosize="{ minRows: 5, maxRows: 6}"
                placeholder="描述"
                v-model="desInfo"
                style="margin-top:10px;width:95%">
            </el-input>
            <small style="margin-top:10px;display:block;color:#778899">列出房内配备的特殊便利设施，比如极速无线网络、停车场等。同时，告诉大家房源所在街区有何魅力。</small>
            </div>
            </div>

        </div>

            <div style="margin-top:8%">
        <h1>标签</h1>
        <i style="display:inline-block;font-size:1.5em;color:#196ebe" class="iconfont icon-biaoqian_huaban1"></i>
        <small style="margin-left:10px;margin-top:10px;margin-bottom:15px;display:inline-block;">给你的房源添加标签吧</small>
        <el-checkbox-group v-model="stayTags" size="medium">
        <el-row v-for="r in rowNums" :key="r" :gutter="20">
          <el-col :span="6"
            ><el-checkbox-button  :ref="rowItems * (r - 1)"  style="width:120px" :label="tagList[rowItems * (r - 1)]">
            </el-checkbox-button></el-col
          >
          <el-col :span="6"
            ><el-checkbox-button :ref="rowItems * (r - 1)+1" v-if="rowItems * (r - 1) + 1<tagList.length" :label="tagList[rowItems * (r - 1) + 1]" style="width:120px">
            </el-checkbox-button></el-col
          >
          <el-col :span="6"
            ><el-checkbox-button :ref="rowItems * (r - 1)+2" v-if="rowItems * (r - 1) + 2<tagList.length" :label="tagList[rowItems * (r - 1) + 2]"  style="width:120px">
            </el-checkbox-button></el-col
          >
          <el-col :span="6"
            ><el-checkbox-button  :ref="rowItems * (r - 1)+3" v-if="rowItems * (r - 1)+3 <tagList.length" :label="tagList[rowItems * (r - 1)+ 3]" style="width:120px">
            </el-checkbox-button></el-col
          >
        </el-row>
        </el-checkbox-group>
            </div>
      </div>

      <div style="display:inline-block;">
        <el-card id="help" class="box-card">
          <i class="el-icon-s-opportunity" style="font-size:2em;color:orange"></i>
          <h3>描述你的房源</h3>
          <p>
            描述房源特色，列出房内配备的特殊便利设施，比如极速无线网络、停车场等。
            <br/>
            同时，告诉大家房源所在街区有何魅力。
            
          </p>
        </el-card>
      </div>

      <!--页尾-->
      <div style="border-top:1px solid #000;" id="footer">
        <el-button type="text" style="margin-top:10px;color:#63aaf1;font-weight:bolder" @click="backPage">返回</el-button>
        <el-button style="margin-top:10px;color:white;float:right;display:inline-block;margin-right:10px;background-color:#63aaf1" @click="nextPage">下一个</el-button>
      </div>
    </div>

  </div>
</template>

<style scoped>
@import "https://at.alicdn.com/t/font_2666220_qwnhywq3ivd.css";
#header {
  text-align: left;
  height: 60px;
  background-color: white;
}

#help{
  width:300px;
  height:300px;
  margin-top:30%;
  text-align: left;
  color: #909399;
}
div::-webkit-scrollbar {
  width: 0;
}

#workspace {
  display: inline-block;
  float:left;
  padding: 20px 10px 20px 150px;
  background-color: white;
  width: 600px;
  height: 480px;
  text-align: left;
     overflow:auto;

}
#footer {
  display: absolute;
  padding: 0 10px 0 150px;
  background-color: white;
  width: 600px;
  height: 80px;
  text-align: left;
  float:left;
}

#mymain {
  height: 600px;
  right: 0 !important;
  background-color: #b2d2f1;
}

.el-row {
  margin-bottom: 20px;

}

.el-col {
  border-radius: 4px !important;
}
.bg-purple-dark {
  background: #99a9bf;
}

.row-bg {
  padding: 10px 0;
  background-color: #f9fafc;
}

.el-divider {
    background-color: black;
    position: absolute;
    width:600px;
    bottom:10px
}
</style>

<script>
import {getStayTagList} from '@/api/stay';

export default {
    data:function(){
        return {
            name:'', // 房源名称
            desInfo:'', //描述
            stayTags:[], //房源标签

            rowItems:4, //每行个数
            tagList: [
                "亲子乐园",
                "可以做饭",
                "告白圣地",
                "山间别墅",
                "环境宜人",
                "自助烧烤",
                "超赞房东",
                "近地铁站"
            ], //标签列表
            
        }
    },

    created(){
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
    },

    mounted(){
      if(localStorage.getItem('stayName')){
      try {
        console.log('从浏览器获取房源昵称');
        this.name = JSON.parse(localStorage.getItem('stayName'));
      } catch(e) {
        console.log(e);
        localStorage.removeItem('stayName');
      }
    }

    if(localStorage.getItem('stayChars')){
      try {
        console.log('从浏览器获取房源类型');
        this.desInfo = JSON.parse(localStorage.getItem('stayChars'));
      } catch(e) {
        console.log(e);
        localStorage.removeItem('stayChars');
      }
    }

    if(localStorage.getItem('stayTags')){
      try {
        console.log('从浏览器获取房源类型');
        this.stayTags = JSON.parse(localStorage.getItem('stayTags'));
      } catch(e) {
        console.log(e);
        localStorage.removeItem('stayTags');
      }
    }
    
      console.log('数据库加载房源tag列表');
      getStayTagList().then(res=>{
          if(res.errorCode==200){
            this.tagList=res.data.tagList;
            console.log('房源tag获取成功！');
          }
          else{
            console.log('房源tag获取失败！');
            this.tagList=['住宅','别墅','旅社','农家乐','乡村小屋','客栈','平房'];
          }
      })
    

    },
    beforeRouteLeave(to,from,next){
    console.log('当前路由跳转to:',to.path.substr(0,15));
          if(to.path.substr(0,15)!='/become-a-host/'){
          console.log('当前路由跳转to:',to);
          this.clearStorage();
  }
  next();

},

    computed: {
    rowNums: function () {
      return Math.ceil(this.tagList.length / this.rowItems);
    },
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
     
      
      nextPage:function(){
        if(this.name=='' ||this.desInfo==''||this.stayTags.length==0){
          console.log('信息不完善！');
        this.$message({
          message:'请先完成本页面信息填写！',
          type:'warning',
          duration:1500,
          center:true
        })
          return ;
        }
        const parsed = JSON.stringify(this.name);
        localStorage.setItem('stayName', parsed);

        const parsed1 = JSON.stringify(this.desInfo);
        localStorage.setItem('stayChars', parsed1);

        const parsed2 = this.stayTags;
        
        localStorage.setItem('stayTags', parsed2);


        this.$router.push('/become-a-host/roomImg')
      },

      backPage:function(){
        this.$router.go(-1);
      }
    }
}
</script>