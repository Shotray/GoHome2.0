<template>
  <div style="width: 100%;">
    <div class="title">
      <el-link :underline="false" style="height: 30px; float: left;" @click="handleBackClicked">
        <el-image src="https://oliver-img.oss-cn-shanghai.aliyuncs.com/img/7d754360eef21afdb2980c794875d420.png"
                  alt="返回" style="height: 30px; width: 30px; float:left;"></el-image>
      </el-link>
      <span style="float: left;" :loading="payLoading">确认并支付</span>
    </div>
    <el-card class="box-card" shadow="hover" :header-style="{}" style="width:70%; margin-bottom: 5%; margin-left: 15%;">
      <div slot="header" class="clearfix">
        <h2 style="font-family: 'FZHua'">【{{ data.stayName }}】{{ data.stayDescription }}</h2>
        <!--      <el-button style="float: right; padding: 3px 0" type="text">操作按钮</el-button>-->
      </div>
      <div class="myInfo">
        <el-container>
          <el-main style="width: 100px;">
            <img :src="thisRoom.roomImage" :alt="thisRoom.roomImage" width="400px"
                 style="border-radius:10px;">
          </el-main>
          <el-aside style="width: 40%;
        box-shadow: rgba(0, 0, 0, 0.05) 0px 1px 2px 0px;">
            <div style="margin:5px 0 5px 0; font-family:'Lato-Bold','FZHeiBJW';">
              <h3 class="infos"><i class="iconfont icon-home2" id="myIcon"></i>房间{{ thisRoom.id }}</h3>
            </div>
            <el-divider><i class="el-icon-star-off"></i></el-divider>
            <b style="font-family: 'FZHeiBJW'">{{ data.characteristic }}</b>
            <div id="ratings" style="margin-top: 2%;">
              <el-rate v-if="commentsReady"
                       v-model="comments.ratings" disabled show-score text-color="#ff9900"
                       score-template="{value}"></el-rate>
            </div>
            <span style="font-family: 'FZHeiBJW'">{{ data.hostLevel }}</span>
            <el-divider><i class="el-icon-s-flag"></i></el-divider>
            <div style="margin:5px 0 5px 0; font-family:'Lato-Bold','FZHeiBJW';">
              <i class="el-icon-crop"></i>
              <span>房间面积<strong class="myVal">{{ thisRoom.area }}</strong>m<sup>2</sup></span>
            </div>
            <div style="margin:5px 0 5px 0; font-family:'Lato-Bold','FZHeiBJW';">
              <i class="el-icon-toilet-paper"></i>
              <span>独享卫生间<strong class="myVal">{{ thisRoom.bathroom }}</strong>个</span>

            </div>
            <div style="margin:5px 0 5px 0; font-family:'Lato-Bold','FZHeiBJW';">
              <i class="el-icon-user"></i>
              <span>最多<strong class="myVal">{{ thisRoom.roomCapacity }}</strong>人</span>
            </div>
            <div style="margin:5px 0 5px 0; font-family:'Lato-Bold','FZHeiBJW';">
              <i class="el-icon-wallet"></i>
              <span>价格<strong class="myVal">{{ thisRoom.price }}</strong>/晚</span>
            </div>
              <i class="el-icon-alarm-clock"></i>
            <span v-for="(bed, index) in thisRoom.beds" :key="index" style="font-family:'Lato-Bold','FZHeiBJW'">
              {{ bed.bedType }} <strong class="myVal">{{ bed.num }}</strong>张
            </span>
          </el-aside>
        </el-container>
      </div>
    </el-card>
    <el-card class="box-card" shadow="hover" :header-style="{}" style="width:70%; margin-bottom: 5%; margin-left: 15%;">
      <div slot="header" class="clearfix">
        <h2 style="font-family: 'FZHua'">您的订单</h2>
      </div>
      <div class="myInfo">

        <el-container>
          <el-main style="width: 50%; margin:0px; padding:0px">
            <div slot="header">
              <h2 style="margin: 10px;">您的行程</h2>
            </div>

            <div>
              <h3 align="center" style="font-family:'Lato-Bold','FZHeiBJW'; margin-top:2%"><i class="el-icon-info"></i>订购信息</h3>
              <p align="left" style="font-family:'Lato-Bold','FZHeiBJW'; margin-left: 10%"><i class="el-icon-date"></i>日期</p>
              <p align="center" style="font-family:'Lato-Bold','FZHeiBJW';">{{ bookDate[0] }} 至 {{ bookDate[1] }}</p>

              <el-dialog
                  title="房客人数"
                  style="font-family: 'FZHua'"
                  :visible.sync="changeNumDialogVisible"
                  width="30%">
                <!--    &lt;!&ndash; :before-close="handleClose" <span>这是一段信息</span>&ndash;&gt;-->
                <span style="float: left; font-family:'Lato-Bold','FZHeiBJW';">房客人数</span>
                <el-input-number
                    v-if="dataReady===true"
                    v-model="changedPeopleNum"
                    :min="1"
                    :max="thisRoom.roomCapacity"
                    label="人数" size="small" style="float: right;"></el-input-number>
                <span slot="footer" class="dialog-footer">
                      <el-button @click="changeNumDialogVisible = false" style="font-family:'Lato-Bold','FZHeiBJW';">取 消</el-button>
                      <el-button type="primary" @click="handleSubmit(1)" style="font-family:'Lato-Bold','FZHeiBJW';">确 定</el-button>
                    </span>
              </el-dialog>

              <p align="left" style="font-family:'Lato-Bold','FZHeiBJW'; margin-left: 10%"><i class="el-icon-s-custom"></i>人数</p>
              <p align="center" style="font-family:'Lato-Bold','FZHeiBJW';">{{peopleNum}}位房客</p><br>
              <el-button align="center" style="font-family:'Lato-Bold','FZHeiBJW';" @click="handleChangeNumOpen">编辑人数</el-button>
            </div>
          </el-main>
          <el-divider direction="vertical"></el-divider>
          <el-aside style="width: 50%">
            <h3 align="center" style="font-family:'Lato-Bold','FZHeiBJW'; margin-top:2%"><i class="el-icon-money"></i>价格详情
            </h3>
            <div>
              <p align="left" style="font-family:'Lato-Bold','FZHeiBJW'; margin-left: 10%">原始价格</p>
              <p align="right" style="font-family:'Lato-Bold','FZHeiBJW'; margin-right: 10%">￥{{ thisRoom.price }} x
                {{ day_count }}晚</p>
            </div>
            <div>
              <p align="left" style="font-family:'Lato-Bold','FZHeiBJW'; margin-left: 10%">服务费</p>
              <p align="right">
              <p align="right" style="font-family:'Lato-Bold','FZHeiBJW'; margin-right: 10%">￥{{ service_price }}</p>
            </div>
            <div>
              <p style="font-family:'Lato-Bold','FZHeiBJW'; margin-left: 10%; float:left">礼券优惠</p>
              <p align="right" style="margin-right: 10%"><el-button style="font-family:'Lato-Bold','FZHeiBJW'; margin-top:10px" @click="chooseCoupon">选择礼券</el-button></p>
              <p align="right" style="font-family:'Lato-Bold','FZHeiBJW'; margin-right: 10%">-￥{{ coupon_price }}</p>

              <el-dialog style="font-family: 'FZHua'"
                  title="可使用的礼券"
                  :visible.sync="selectCouponVisible"
                  width="30%">
                <!-- 在这个地方调用获得礼券API,得到可使用礼券进行展示 -->
                <!-- 分页后,展示礼券数据，即coupon_information -->
                <div  v-for="(coupon,index) in coupon_information" :key="index">
                    <el-card class="box-card" style = "margin-bottom:10px;" @click.native="clickCouponCard(coupon.couponId,coupon.couponAmount)">

                      <p align="left" style="font-family:'Lato-Bold','FZHeiBJW'; margin-left: 10%">礼券名称</p>
                      <p align="right" style="font-family:'Lato-Bold','FZHeiBJW'; margin-right: 10%">{{coupon.couponName}}</p>
                      <p align="left" style="font-family:'Lato-Bold','FZHeiBJW'; margin-left: 10%">
                        <i class="el-icon-date"></i>礼券使用日期</p>
                      <p align="right" style="font-family:'Lato-Bold','FZHeiBJW'; margin-right: 10%">{{coupon.couponStartDate}}至
                        {{coupon.couponEndDate}}
                      </p>
                      <p align="left" style="font-family:'Lato-Bold','FZHeiBJW'; margin-left: 10%">礼券使用金额限制</p>
                      <p align="right" style="font-family:'Lato-Bold','FZHeiBJW'; margin-right: 10%">满￥{{coupon.couponLimit}}可以使用</p>
                      <p align="left" style="font-family:'Lato-Bold','FZHeiBJW'; margin-left: 10%">礼券优惠金额</p>
                      <p align="right" style="font-family:'Lato-Bold','FZHeiBJW'; margin-right: 10%; font-size: x-large;">￥{{coupon.couponAmount}}</p>
                    </el-card>
                  </div>
                <el-pagination
                :page-size="coupon_pageSize"
                :page-count="coupon_totalPage"
                @current-change="handleCurrentChange"
                @prev-click="prevCurrentChange"
                @next-click="nextCurrentChange"
                layout="prev, pager, next"
                :total="coupon_pageSize*coupon_totalPage">
                </el-pagination>
              </el-dialog>

            </div>
            <div>
              <p align="left" style="font-family:'Lato-Bold','FZHeiBJW'; margin-left: 10%">总价</p>
              <p align="right">
              <p align="right" style="font-family:'Lato-Bold','FZHeiBJW'; margin-right: 10%">￥{{ total_price }}</p>
            </div>
          </el-aside>
        </el-container>
      </div>
    </el-card>

    <el-card class="box-card" shadow="hover" :header-style="{}" style="width:70%; margin-bottom: 5%; margin-left: 15%;">
      <span style="float: left;font-family:'Lato-Bold','FZHeiBJW';">选择以下按钮即代表我同意</span>
      <el-link :underline="false" style="float: left;"><strong style="text-decoration:underline;
       font-family:'Lato-Bold','FZHeiBJW';font-size: 16px;" @click="houseRulesDialogVisible = true;">房屋守则</strong>
      </el-link>
      <el-dialog
          title="入住须知"
          style="font-family: 'FZHua'"
          :visible.sync="houseRulesDialogVisible"
          width="30%"
          >
        <el-row :gutter="20">
          <svg viewBox="0 0 24 24" role="presentation" aria-hidden="true" focusable="false"
                style="height: 20px; width: 20px; display: inline-block; fill: currentcolor;">
            <path
                d="m15.6 12.6h-1.4c-.3 0-.5.2-.5.5v6c0 .5-.4.8-.8.8-.5 0-.8-.4-.8-.8v-1.9c0-.3-.2-.5-.5-.5h-.2c-.3 0-.5.2-.5.5v1.9c0 .5-.4.8-.8.8-.5 0-.8-.4-.8-.8v-3.4l4.6-4.6h2v-2l.6-.6v2.6c.4 0 .8.4.8.8s-.3.8-.8.8v1.3c0 .2-.1.3-.3.3s-.3-.1-.3-.3zm-10.2 5.3.6-.6s4.2-4.3 12.6-12.8c0 0 0 0 .1-.1.2-.2.2-.5 0-.8-.2-.2-.5-.2-.7 0l-.2.2c-.4-.6-1-1-1.7-1-1.1 0-2 .9-2 2.1 0 .6.3 1.3.8 2l-1.4 1.4c-.1-.9-.9-1.6-1.9-1.6-1.1 0-1.9.9-1.9 1.9s.7 1.8 1.6 1.9l-.4.4c-.6-.1-1.2-.2-1.7-.6l-1.8-1c-.4-.2-.8-.1-1 .3s-.1.8.3 1.1l2.2 1.3c.2.1.4.4.5.6l-4.4 4.5c-.2.2-.2.5 0 .8.2.2.5.2.7 0z"></path>
          </svg>
          <span style="font-size: 16px; font-family: FZHeiBJW">不适合儿童和婴幼儿入住</span>

          <!--              </li>-->
        </el-row>
        <el-row :gutter="20">
          <svg viewBox="0 0 24 24" role="presentation" aria-hidden="true" focusable="false"
                style="height: 20px; width: 20px; display: inline-block; fill: currentcolor;">
            <path
                d="m6.32874849 11.6781574c.75542549 0 1.34774775-.5410186 1.34774775-1.5525901 0-.82366353-.59232226-2.99008648-2.1195616-2.99008648-.91618752 0-1.67395421.77433766-1.67395421 1.68569158 0 1.12509931 1.30014346 2.856985 2.44576806 2.856985m12.63854941-4.52701758-2.1484364 2.15546165c-.0741378.29595514-.1131577.58016603-.1131577.81896583 0 1.0115715.5923223 1.5525901 1.3485281 1.5525901 1.1979113 0 2.4457681-1.8039171 2.4457681-2.85463615 0-.86202807-.6797269-1.59721822-1.5327021-1.67238143m-3.9456935 3.95859578-8.40176737 8.4292409c.37381076.7351901 1.01139611 1.2190533 2.0337178 1.2190533 1.63337347 0 2.50039597-1.4727292 3.31044927-1.569032 1.0956792-.1299698 1.8339359 1.8109636 3.6990877 1.8109636 1.9018304 0 2.5269294-1.7154437 2.5269294-3.6297567 0-2.6510691-1.1979113-5.0735168-3.1684168-6.2604691m-4.869685-1.67238146c1.0652436 0 1.3813049-1.55493892 1.3813049-2.59469666 0-1.45237246-.5267688-2.75363554-1.90651288-2.75363554-.93491708 0-1.82144947.7281436-1.82144947 1.93623562 0 1.12353341 1.11128708 3.41209658 2.34665745 3.41209658m9.3031274-4.43462942-14.95633216 15.00445588c-.11393814.1143107-.2637746.1714661-.41361107.1714661-.14983646 0-.29967292-.0571554-.41361106-.1714661-.22865668-.2294044-.22865668-.6013057 0-.83071l2.56829059-2.5766889c.31528088-3.0887381 2.37319102-6.2589031 6.0067252-6.2589031.0741378 0 .1428129.0140931.2153899.0164419l1.1479658-1.15093664c-.5704711-.46663827-.7608883-1.56041957-.7608883-2.36059458 0-1.45158951.5244276-2.75676734 1.9072933-2.75676734.8880932 0 1.8198887.69134495 1.8198887 1.93232087 0 .06811666-.0046824.14327987-.0140472.22314078l2.0649337-2.07246894c.2286567-.22940438.5993459-.22940438.8280026 0 .2286566.22940438.2286566.60130568 0 .83071007">
            </path>
          </svg>
          <span style="font-size: 16px; font-family: FZHeiBJW">不允许携带宠物</span>

        </el-row>
        <el-row :gutter="20">
          <svg viewBox="0 0 24 24" role="presentation" aria-hidden="true" focusable="false"
                style="height: 20px; width: 20px; display: inline-block; fill: currentcolor;">
            <path
                d="m19.2824314 18.7231393c.0580699.2873308-.1238824.5683982-.4080378.6278999l-3.0614452.6380779c-.2841554.0587188-.5621167-.1260497-.6209608-.4125976-.0518758-.2536653.0851692-.5088964.3244171-.6028466.4242975-.1675443.9507979-.3202133.8586603-.7711738l-.7371006-3.6162966c0-.0007829 0-.0015659-.0007743-.0015659-.0766522-.3640567-.2965436-.4893235-.481593-.5543057-.9012449-.274804-1.4966549-.9700349-1.8435258-1.8547319-.0394876-.1009963-.0712324-.2145194-.1083972-.3186474l4.3838905-4.43209681c.2632502 1.21900277.4862386 2.56248945.5226291 3.74860971.0309706.9363695-.2369252 1.8038422-.942281 2.4231301-.1672413.1511031-.3228686.3554446-.2477649.7249817l.7378749 3.6131649c.0913633.4493948.6372204.3812809 1.0909399.3648397.2562818-.010178.4815931.1698931.5334688.4235584m-10.22727104.7719568c-.05884417.2881137-.33680543.4728823-.62096082.4141634l-2.64721325-.5519569 2.30885929-2.3354433-.22453695 1.0999992c-.09136331.4517435.43513713.6044125.86020881.7719568.23847373.0939502.37474443.3483984.32364292.6012808m3.06144524-12.16419143c.0960089.60284659.2206656 1.26362908.367776 1.93302366l-.9748001.98569327c.2578304-.99195661.4622365-2.01522997.6070241-2.91871693m7.2138303-4.15885852c-.2268598-.22939487-.5946358-.22939487-.8214956 0l-1.5918895 1.60967868c-.1231082-.42825595-.2253112-.75786428-.2833811-.94263285-.0309707-.09629887-.1285281-.15423477-.2268598-.13387891l-4.0904439.85416315-4.47602798-.93402075c-.0983317-.02113878-.19666341.03679713-.22685975.13387891-.26325022.82832687-1.38980632 4.52369815-1.4772983 7.33280662-.03097062.9340208.23847373 1.8054081.9430552 2.424696.16724132.1503202.32286865.3546617.24776491.7241988l-.06194122.3006404-3.59491405 3.6350866c-.22685975.2293949-.22685975.6004978 0 .8298927.11381701.1150889.26247596.1722419.41113491.1722419.14865894 0 .29654363-.057153.41036063-.1722419l14.83879555-15.00383373c.2260855-.22939487.2260855-.60128075 0-.83067562"></path>
          </svg>
          <span style="font-size: 16px; font-family: FZHeiBJW">不允许举办派对和活动</span>
        </el-row>
        <el-row>
          <svg viewBox="0 0 24 24" role="presentation" aria-hidden="true" focusable="false"
                style="height: 20px; width: 20px; display: inline-block; fill: currentcolor;">
            <path
                d="m4.50336409 19.0040004c-.00078342.0015657-.00313367.0015657-.00470051.0031313l-.00470052.0046968s-.00078342 0-.00078342-.0007828c-.11359577.1080272-.25931174.165172-.4050277.165172-.15041648 0-.30083296-.0579276-.41599558-.1722173-.22954181-.2293621-.22954181-.6011949 0-.830557l.29456561-.294335-.01645181-2.8055764c-.00156683-.4344572.36272308-.7225298.70977777-.7248782l2.85791311-.0172218 6.46007446-6.45501741c-.0195855-.03522626-.0321202-.08141181-.0564062-.11037563-.2311087-.26928523-.4692681-.29042099-1.274623-.2262309-.6000991.04618554-1.3490478.10333037-2.0846784-.21605443-2.20532494-.96285126-2.1630203-3.12339554-2.14421824-4.0478893.00078341-.04618555.00235025-.0790634.00235025-.1189865.00783419-.04148871.03917096-.07436656.0814756-.07436656h2.10974779c.0446549 0 .0861761.03679188.0830424.10019915-.0070507 1.0340866-.0329036 1.6853811.7732348 2.03607815.2162236.09471951.5985322.0665385 1.0051267.03522626.8507933-.06732131 2.1371675-.16125801 3.1438612.98476982l3.0694362-3.06703351c.2295419-.22936213.601666-.22936213.8312078 0s.2295418.60119493 0 .83055706zm15.99663591-7.7161177v1.8975215c0 .10881-.0877429.1972671-.1958548.1972671-.2655791 0-1.6702496.0007828-1.9060588.0007828-1.0059102 0-3.033399-.0939367-3.9547-2.1401913-.0571896-.1275974-.0869595-.258326-.1237802-.3882718l2.1042639-2.10418222c.0023502.15734399.0031337.31077394.0015668.45715865-.0023502.25519472-.0117513.85717247.0908767 1.08966577.3055334.6779099.9024988.7796747 2.0235717.7929824h1.7642599c.1081119 0 .1958548.0884571.1958548.1972671zm-4.3088054 7.2049455-.0156684-2.7758297 2.8774986-.0133077.0125347 2.7758297zm4.283736-3.5187125c-.0023503-.395317-.3227687-.7154846-.7199622-.7154846l-8.8957246.0532308-5.69310709 5.6878676 14.62408549-.0023484c.4261801-.0093936.7113446-.3483486.7089944-.7240954l-.0086176-1.661897z"></path>
          </svg>
          <span style="font-size: 16px; font-family: FZHeiBJW">禁止吸烟</span>
        </el-row>
      </el-dialog>
      <span style="float: left; font-family:'Lato-Bold','FZHeiBJW';">,</span>
      <el-link :underline="false" style="float:left;">
        <strong style="text-decoration: underline; font-size: 16px; font-family:'Lato-Bold','FZHeiBJW';" @click="covidRulesDialogVisible=true;">
          归宿的社交距离准则和其他新冠肺炎疫情相关准则</strong>
      </el-link>
      <el-dialog
        style="font-family: 'FZHua'"
        title="新冠疫情相关准则"
        :visible.sync="covidRulesDialogVisible"
        width="55%">
        <section>
          <main>
            <section>
              <div>
                <div>
                  <div style="margin-bottom:24px">
                    <h1>
                      <div>
                        <div>
                          适用于归宿房东和房客的卫生和安全准则是什么？
                        </div>
                      </div>
                    </h1>
                  </div>
                </div>
              </div>
              <div>
                <div>
                  <div style="font-family: FZFWZhu">
                    <p>在新冠肺炎疫情肆虐期间，请务必重视健康与安全。根据
                      <a href="https://www.who.int/emergencies/diseases/novel-coronavirus-2019" target="_blank">
                        世界卫生组织
                      </a>
                      和
                      <a href="https://www.cdc.gov/coronavirus/2019-ncov/" target="_blank">
                        美国疾控中心
                      </a>
                      的指导意见，我们制定了一系列针对归宿房东/体验达人和房客/体验参与者的指导方针。您还应当继续留意
                      <a href="http://www.gov.cn/fuwu/bm/wsjkw/index.htm" target="_blank">
                        政府的旅行建议
                      </a>
                      ，遵守所有国家和地区的指导意见。
                    </p>
                    <p>归宿已制定了指导意见和计划来解决卫生和安全问题，但这些措施无法消除所有风险。如果您属于高风险人群（例如年龄在 65
                      岁以上，或患有糖尿病或心脏病等病症），建议您在决定通过归宿预订住宿或体验时获取专业指导，同时采取额外的预防措施。</p>
                    <p>
                      归宿全球社区和我们周围的世界一样多姿多彩、独一无二、充满生机。公平原则将我们紧密联系在一起，让我们每一个人都能完全融入归宿社区，并拥有真切的归属感。面对这前所未有的形势，让我们竭尽全力、相互支持，保障社区的安全与健康。
                    </p>
                  </div>
                </div>
                <div>
                  <div>
                    <h2>如果您最近接触过新冠肺炎病毒，或已经出现新冠肺炎症状，请勿出行或出租房源</h2>
                    <p style="font-family: FZFWZhu">为保护归宿社区成员的健康和安全，归宿要求，如果出现如果出现以下情况，
                      <strong>房东</strong>（以及入住前或入住期间可能进入房源的所有人员）不得进入房源或与房客互动，
                      <strong>房客</strong>也不应入住房源：
                    </p>
                    <ul style="font-family: FZFWZhu">
                      <li>在过去 30 天内，您感染了新冠肺炎或病毒检测结果呈阳性</li>
                      <li>您怀疑自己生病或接触到新冠病毒，且正在等待检测结果以确诊或排除感染</li>
                      <li>您正在出现相关症状或担心可能感染新冠肺炎</li>
                      <li>您在过去 14 天内与已确认或怀疑感染新冠肺炎的个人有密切、持续的联系*</li>
                    </ul>
                    <p style="font-family: FZFWZhu">如果房东和房客必须互动，或者存在预订无关人员可以进入的公共区域，请按照
                      <a href="https://www.cdc.gov/coronavirus/2019-ncov/prevent-getting-sick/diy-cloth-face-coverings.html"
                          target="_blank">
                        美国疾控中心
                      </a>或
                      <a href="https://www.ecdc.europa.eu/sites/default/files/documents/COVID-19-use-face-masks-community.pdf"
                          target="_blank">
                        欧洲疾控中心
                      </a>
                      等监管机构或官方的建议，时刻佩戴防护口罩或面罩。
                    </p>
                  </div>
                </div>
                <div>
                  <div>
                    <h2>请务必定期洗手
                    </h2>
                    <p style="font-family: FZFWZhu">请务必经常洗手，尤其是当您与预订无关人员接触，以及触摸共用空间或公共区域的表面和用具后
                    </p>
                    <ul style="font-family: FZFWZhu">
                      <li>用肥皂和清水洗手至少 20 秒
                      </li>
                      <li>如果无法使用肥皂和清水洗手，请使用酒精含量至少达到 60% 的免洗消毒液，方法是将其涂满双手并搓揉，直至感到干爽为止
                      </li>
                    </ul>
                  </div>
                </div>
                <div>
                  <div>
                    <h2>适用于所有房东的准则</h2>
                    <p style="font-family: FZFWZhu">此外，我们建议所有房东采取以下行动：</p>
                    <ul style="font-family: FZFWZhu">
                      <li>请遵守当地有关房源内聚集人数的限制准则
                      </li>
                    </ul>
                  </div>
                </div>
                <div>
                  <div>
                    <h2>针对提供独立房间和共用空间的房东的补充准则</h2>
                    <p style="font-family: FZFWZhu">提供独立房间或共用空间房源的房东还应：</p>
                    <ul style="font-family: FZFWZhu">
                      <li>设置房客人数上限，以便在所有公共区域保证社交距离</li>
                      <li>限制房客可以进入的空间，减少您与房客不必要的接触，降低感染风险</li>
                      <li>在确保安全的情况下，在入住期间根据清洁规程对公共区域进行通风</li>
                      <li>尽可能频繁地清洁和消毒公共区域（如卫生间和厨房）</li>
                    </ul>
                    <p style="font-family: FZFWZhu">有些政府可能对独立房间或共用房间的规定了出租限制措施，也可能要求提供独立房间或共用空间的房东遵守额外的义务或要求。请务必查看并遵守您所在地区政府和/或卫生部门发布的任何其他安全及清洁指南。
                    </p>
                  </div>
                </div>
              </div>
            </section>
          </main>
        </section>
      </el-dialog>
      <span style="float: left; font-family:'Lato-Bold','FZHeiBJW';">,</span>
      <el-link :underline="false" style="float:left; " @click="cancelRulesDialogVisible=true;">
        <strong style="text-decoration: underline; font-size: 16px; font-family:'Lato-Bold','FZHeiBJW';">取消政策</strong>
      </el-link>
      <el-dialog
        style="font-family: 'FZHua'"
        title="取消政策"
        :visible.sync="cancelRulesDialogVisible"
        width="30%">
        <div>
          <span style="display: block; font-family: FZFWZhu">我们的特殊情况政策不适用于新冠肺炎疫情造成的旅行中断。</span>
        </div>
      </el-dialog>
      <span style="float: left; font-size: 16px; font-family:'Lato-Bold','FZHeiBJW';">,我也同意支付以上所示的总金额(含服务费)。</span>
      <div style="display: block;margin-top: 3%;">
        <el-button type="primary" style="font-size: 16px; font-family:'Lato-Bold','FZHeiBJW';" plain @click="handleBookButtonClicked">预定并支付</el-button>
      </div>
    </el-card>


  </div>
</template>
<script>


// api
import {getStayDetails, getComments} from "@/api/stay.js";
import {AddOrder, UseCouponForOrder} from '@/api/order.js';
import {GetCustomerCouponInfo} from "@/api/coupon.js";


export default {
  name: "PaymentPage",
  data() {
    return {
      //房客人数
      peopleNum: 1,

      //
      changeNumDialogVisible: false,
      changeDateDialogVisible: false,
      houseRulesDialogVisible: false,
      covidRulesDialogVisible: false,
      cancelRulesDialogVisible: false,
      selectCouponVisible: false,


      changedPeopleNum: 1,
      changedDate: [0, 0],
      bookDate: new Date,

      //房源的data数据
      data: [],

      //预定的房间的信息
      thisRoom: "",

      //表示是否加载完成
      dataReady: false,
      priceReady: false,
      commentsReady: false,

      //评论数据信息
      comments: Object,

      //订单是否创建
      orderCreated: false,

      //价格与订购天数
      day_count: 0,
      service_price: 5,
      coupon_price: 0,

      //礼券信息
      coupon_information: [],
      coupon_currentPage: 1,
      coupon_pageSize: 2,
      coupon_totalPage: 0,
      coupon_id:-1,

      payLoading:true,

    }
  },
  methods: {
    //从开始日期到结束日期的天数
    getDaysBetween(startDateString, endDateString) {
      let startDate = Date.parse(startDateString);
      let endDate = Date.parse(endDateString);
      let days = (endDate - startDate) / (1 * 24 * 60 * 60 * 1000);
      return days;
    },

    changeOrderCreated() {
      this.orderCreated = false;
    },

    //点击返回后触发的事件
    handleBackClicked() {
      let stay_id = this.$route.query.stayId;
      this.$router.replace({path: '/StayInfo', query: {stayId: stay_id}});
    },

    //点击预定后触发的时间
    handleBookButtonClicked() {
      let data =
        {
          "memberAmount": this.peopleNum,
          "totalCost": this.total_price,
          "orderStatus": 1,
          "orderStayEntityList":[
            {
              "stayId": this.$route.query.stayId,
              "roomId":this.thisRoom.id,
              "startTime":this.bookDate[0],
              "endTime":this.bookDate[1],
              "moneyAmount":this.day_count*this.thisRoom.price
            }
          ]
        }

      //调用API添加完成订单;
      AddOrder(data).then((response) => {

        //调用API对礼券的状态进行修改;
        if (this.coupon_id !== -1){
          UseCouponForOrder(this.coupon_id).then((response) => {
            console.log("已经使用完了礼券!")
          }).catch((error) => {
            this.$message({
              message: error,
              type: "can't use this coupon!",
            });
            return;
          });
        }

        //添加完成的订单
        console.log(response.data);
        this.orderCreated = true;

        const div = document.createElement('div')
        div.innerHTML = response.data
        document.body.appendChild(div)
        document.forms[0].submit() //重要，这个才是点击跳页面的核心

      }).catch((error) => {
        this.$message({
          message: error,
          type: "order created warning",
        });
        return;
      });

    },

    //修改房客人数
    handleSubmit(type) {
      if (type === 1) {
        // 修改人数
        this.peopleNum = this.changedPeopleNum;
        this.changeNumDialogVisible = false;
      } else if (type === 0) {
        // 修改日期
        // console.log(this.changedDate)
        this.changeDateDialogVisible = false;
        this.bookDate[0] = this.changedDate[0];
        this.bookDate[1] = this.changedDate[1];
        this.$router.replace({
          path: "/PaymentPage", query: {
            stayId: this.stayId,
            roomId: this.room.id,
            startDate: this.bookDate[0],
            endDate: this.bookDate[1]
          }
        });
      }
    },

    //显示改变人数界面
    handleChangeNumOpen() {
      this.changeNumDialogVisible = true;
      this.changedPeopleNum = this.peopleNum;
    },

    handleChangeDateOpen() {
      this.changeDateDialogVisible = true;
      this.changedDate[0] = this.$route.query.startDate;
      this.changedDate[1] = this.$route.query.endDate;
    },

    getCouponListByPage(price, currentPage, pageSize){
      GetCustomerCouponInfo(price,currentPage,pageSize).then(response=>{

        this.coupon_information = response.data.couponInfo;
        this.coupon_totalPage = response.data.totalPage;
        this.selectCouponVisible = true;

      })
    },
    //显示礼券界面
    chooseCoupon:function(){
      console.log("获取礼券信息")
      this.getCouponListByPage(this.total_price,this.coupon_currentPage - 1,this.coupon_pageSize)
    },
    //点击卡片选择礼券
    clickCouponCard:function(couponId, couponAmount){
        console.log("点击了这个礼券卡片")
        this.coupon_price = couponAmount;
        this.coupon_id = couponId;
        this.selectCouponVisible = false;
    },
    //礼券分页
    handleCurrentChange(val){
      this.coupon_currentPage = val;
    },
    prevCurrentChange(val){
      this.coupon_currentPage = val;
    },
    nextCurrentChange(val){
      this.coupon_currentPage = val;
    }


  },
  computed: {
    pickerOptions() {
      if (this.dataReady) {
        let that = this;
        return {
          disabledDate(time) {
            const today = new Date().toLocaleTimeString();
            // 禁用今天之前的日期
            let disable = time < new Date(today);
            // 禁用后端返回的禁止日期
            that.thisRoom.unavailable.forEach((item) => {
              disable =
                  disable || (time.getTime() > new Date(item.startDate).getTime() - 8.64e7 &&
                  time.getTime() < new Date(item.endDate).getTime());
            });
            return disable;
          }
        }
      }
    },
    total_price:function() {
      return this.thisRoom.price * this.day_count + this.service_price - this.coupon_price
    }
  },
  created() {

    this.changedDate[0] = this.$route.query.startDate;
    this.bookDate[0] = this.$route.query.startDate;
    this.changedDate[1] = this.$route.query.endDate;
    this.bookDate[1] = this.$route.query.endDate;

    let stayId = this.$route.query.stayId;
    let roomId = this.$route.query.roomId;

    getStayDetails(stayId).then((response) => {

      this.data = response.data;

      for (let room in this.data.rooms) {
        if (this.data.rooms[room].id == roomId) {
          this.thisRoom = this.data.rooms[room];
        }
      }
      this.dataReady = true;
      this.payLoading=false;
    })
        .catch((error) => {
          this.$message({
            message: error,
            type: "warning",
          });
          return;
        });

    getComments(stayId)
        .then((response) => {
          this.comments = response.data;
          this.commentsReady = true;

        })
        .catch((error) => {
          this.$message({
            message: error,
            type: "warning",
          });
          return;
        });

    let startDate = this.bookDate[0];
    let endDate = this.bookDate[1];
    this.day_count = this.getDaysBetween(startDate, endDate)

  },
  watch:{
    coupon_currentPage(val,oldVal){
      this.getCouponListByPage(this.total_price,val - 1,this.coupon_pageSize)
    }
  }

}
</script>

<style scoped>
.title {
  display: inline-block;
  margin-left: 10%;
  margin-right: 10%;
  width: 80%;
}

.el-card >>> .el-card__header {
  padding: 0px !important;
  height: 50px;
  margin: 8px;
}

.detail >>> .el-card__header {
  height: 150px;
}

.el-divider--vertical{
  height:40vh;
}
</style>
