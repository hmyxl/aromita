<template>
  <!-- 头部 -->
  <div>
    <!-- 大头部 -->
    <div class="wrap1">
      <div class="wrap">
        <div class="title" v-if="!titleActivity" style="background-color: #121037;">
          <div class="title_word">Contact Us: 1-626-586-3448 (Mon-Fri 9am-6pm PST.)</div>
        </div>
        <div class="title activity" v-if="titleActivity" :style="'background-color:' + activityDetail.top_ad_bgcolor">
          <div class="activityBox">
            <div class="title_word act" :style="'justify-content:'+ titlePosition + '; color:' + activityDetail.top_ad_fontcolor">
              <span class="titleAct" @click="toActity(activityDetail.top_ad_title_url)"><span>{{activityDetail.top_ad_title}}</span></span>
              <span>
                <span class="timeAct"><span v-if="activityDetail.countdown_status === 1">{{countDownList}}</span></span>
                <span v-if="activityDetail.top_ad_detail_url" class="iconFlag" :class="showIconActive" @click="showActivity(showIconActive)"></span>
              </span>
            </div>
            <!--<div class="activityCont" v-if="activityShow">-->
              <!--<img src="../../static/img/title.jpg" alt="">-->
            <!--</div>-->
          </div>
        </div>
        <div class="imgTest" v-if="titleActivity" :style="{top:(activityShow==true?'0':'-550px')}">
          <div class="activityCont">
            <a :href="activityDetail.top_ad_detail_url"><img :src="activityDetail.top_ad_detail_image" alt=""></a>
          </div>
        </div>
        <div>{{Refresh?'':''}}</div>
        <div>{{showHistoryRefresh?'':''}}</div>
        <!-- 中间内容 -->
        <div class="content">
          <div class="contentHd">
            <!-- 左边logo -->
            <div class="LOGO2" @click="handleGoHome">
              <img src="@/assets/LOGO2.png" alt>
            </div>
            <div class="searchBox">
              <!-- 搜索框 -->
              <div class="search" id="search">
                <div class="serInput">
                  <el-input :placeholder="searchInput" v-model="searchVal" @focus="inputFocus()" @keyup.enter.native="searchOver()"></el-input>
                  <p class="search_word" @click="searchOver($event)">Search</p>
                </div>
                <div class="serHistory" v-if="showHistory">
                  <div class="hisTitle"><span v-if="historyList.length>0">Search history</span>
                    <div class="hisClear" v-if="historyList.length>0" @click="clearHistory()">clear search history</div>
                  </div>
                  <div style="padding: 0 10px;">
                    <div class="hisItem" v-for="(his, index) in historyList" :key="index" @click="toSearch(his)">{{his}}</div>
                    <!--<div class="hisItem">Pendants</div>-->
                    <!--<div class="hisItem">nice</div>-->
                    <!--<div class="hisItem">sanick</div>-->
                    <!--<div class="hisItem">Rings</div>-->
                    <!--<div class="hisItem">Pendants</div>-->
                    <!--<div class="hisItem">nice</div>-->
                    <!--<div class="hisItem">clear</div>-->
                  </div>
                  <!--<div class="hisClear" v-if="historyList.length>0" @click="clearHistory()">clear search history</div>-->
                  <div class="hisTitle" style="margin-top: 10px;clear: both;" v-if="hotList.length>0">Others are searching</div>
                  <div style="padding: 0 10px 20px 10px;">
                    <div class="hisItem hot" v-for="(hot, index2) in hotList" :key="index2" @click="toSearch(hot.name)">{{hot.name}}</div>
                    <!--<div class="hisItem hot">Pendants</div>-->
                    <!--<div class="hisItem hot">nice</div>-->
                    <!--<div class="hisItem hot">sanick</div>-->
                  </div>
                </div>
              </div>
              <div class="optionList">
                <!-- 登录 -->
                <div class="login">
                  <div class="login_left" @click="HandleLogin" v-if="!login_status">
                    <img src="@/assets/login.png" alt>
                    <span>{{userName}}</span>
                  </div>
                  <el-dropdown v-if="login_status">
                    <div class="login_right" :title="userName">
                      <router-link to="/myAccount">
                        <span v-if="!userName">Hello,Welcome</span>
                        <span v-if="userName">Hello,{{userName.substring(0,7)}}<span v-if="userName && userName.length>7">...</span></span>
                      </router-link>
                    </div>
                    <div class="dropdown">
                      <el-dropdown-menu slot="dropdown">
                        <el-dropdown-item><router-link to="/myOrder">My Orders</router-link></el-dropdown-item>
                        <el-dropdown-item><router-link to="/myCoupon">My Coupons</router-link></el-dropdown-item>
                        <el-dropdown-item><router-link to="/myPoints">My Points</router-link></el-dropdown-item>
                        <el-dropdown-item><span @click="logout()">Logout</span></el-dropdown-item>
                      </el-dropdown-menu>
                    </div>
                  </el-dropdown>
                </div>
                <!-- 心愿单 -->
                <router-link to="/myWishlist">
                  <div class="wish">
                    <div class="wish_left">
                      <img src="@/assets/wish.png" alt>
                    </div>
                    <div class="wish_right">
                      <p class="wish_word">Wish</p>
                    </div>
                  </div>
                </router-link>
                <!-- 购物车 -->
                <div class="cart" v-on:click="visible">
                  <el-badge :hidden="goodsNum === 0?true:false" :value="goodsNum" :max="99" class="item">
                    <div class="cart_left">
                      <img src="@/assets/cart.png" alt>
                      <div>cart</div>
                    </div>
                  </el-badge>
                </div>
              </div>
            </div>
          </div>
          <nav-com v-if="$route.meta.hasNav"></nav-com>
          <div v-if="!$route.meta.hasNav" style="height: 10px;"></div>
        </div>
      </div>
    </div>
    <!-- 小头部 -->
    <div class="small_header" :style="{top:(headerShow==true?'0':'-60px')}">
      <div class="box">
        <img src="@/assets/LOGO2.png" alt @click="handleGoHome" class="logo">
        <nav-com v-if="$route.meta.hasNav"></nav-com>
        <div v-if="!$route.meta.hasNav" style="height: 20px; width: 1050px;"></div>
        <i class="el-icon-search" @click="searchIcon()" v-if="searchShow == false"></i>
        <div class="login">
          <div class="login_left" @click="HandleLogin" v-if="!login_status">
            <img src="@/assets/login.png" alt>
          </div>
          <el-dropdown v-if="login_status">
            <div class="login_right" :title="userName">
              <router-link to="/myAccount"><span>{{userName}}</span></router-link>
            </div>
            <div class="dropdown">
              <el-dropdown-menu slot="dropdown">
                <el-dropdown-item><router-link to="/myOrder">My Orders</router-link></el-dropdown-item>
                <el-dropdown-item><router-link to="/myCoupon">My Coupons</router-link></el-dropdown-item>
                <el-dropdown-item><router-link to="/myPoints">My Points</router-link></el-dropdown-item>
                <el-dropdown-item><span @click="logout()">Logout</span></el-dropdown-item>
              </el-dropdown-menu>
            </div>
          </el-dropdown>
        </div>
        <div class="cart" v-on:click="visible">
          <el-badge :hidden="goodsNum === 0?true:false" :value="goodsNum" :max="99" class="item">
            <img src="@/assets/cart.png" alt>
          </el-badge>
        </div>
      </div>
      <div class="search_box" v-if="searchShow == true">
        <div class="search">
          <i class="el-icon-search" style="cursor: pointer;" @click="searchOver()"></i>
          <el-input :placeholder="searchInput" v-model="searchVal" @keyup.enter.native="searchOver()"></el-input>
          <i class="el-icon-close" @click="searchIcon()"></i>
        </div>
      </div>
    </div>
    <!-- 购物车缩略版 -->
    <div class="cart_list" v-on:mouseenter="visible" v-on:mouseleave="invisible" :style="{right:(show==true?'0':'-560px')}">
      <div class="cart_title">
        <i class="el-icon-close" @click="invisible"></i>
        <p>Your Cart Has {{this.goodsNum}} Items</p>
      </div>
      <div class="cart_center">
        <ul v-if="goodsListOn.length!=0">
          <li v-for="(item,index) in goodsListOn" :key="index">
            <div class="imgBox">
              <div class="tagBox" v-if="item.activity_type">
                <div class="cheap" v-if="item.activity_type == 1">
                  <div class="cheapLeft"></div>
                  <div class="cheapRight">${{item.activity_price}}</div>
                </div>
                <div class="disPrice" v-if="item.activity_type == 2">%{{parseInt(item.activity_intensity)}} OFF</div>
              </div>
               <img :src="item.sku_image" alt @click="link(item.sku_id,item.product_id)">
            </div>
            <div class="list_detail">
              <p class="detail_title" @click="link(item.sku_id,item.product_id)">{{item.sku_name}}</p>
              <div class="spec_color">
                <p class="size" @mouseenter="attrshow(index)" @mouseleave="attrhidden()">
                  <span v-if="item.sku_attrs"><span v-for="(item1,index1) in JSON.parse(item.sku_attrs)" :key="index1">
                    <span v-if="index1<2">
                      {{item1.attr_name}}:<span style="color: #333;">{{item1.value.attr_value}}</span>;
                    </span>
                    <span v-if="index1>=2">...</span>
                  </span></span>
                </p>
                <span v-if="item.sku_attrs"><p class="sizevisible" v-if="JSON.parse(item.sku_attrs).length>=2" v-show="attrShowindex == index">
                  <span v-for="(item1,index2) in JSON.parse(item.sku_attrs)" :key="index2">
                    {{item1.attr_name}}:<span style="color: #333;">{{item1.value.attr_value}}</span>;
                  </span>
                </p></span>
                <p class="qty"><span>QTY:</span>{{item.goods_count}}</p>
              </div>
            </div>
            <div class="price_del">
              <i class="el-icon-error" @click="delList(item.sku_id)"></i>
              <div class="price">${{((item.activity_price?item.activity_price:item.sku_price)*item.goods_count).toFixed(2)}}</div>
            </div>
          </li>
        </ul>
        <div class="addMoreBtn" v-if="anotherGoodsList.length>0">加购</div>
        <ul v-if="anotherGoodsList.length>0">
          <li style="margin-top:10px;" v-for="(item,index) in anotherGoodsList" :key="index">
            <img :src="item.sku_image" alt style="margin-right:0;" @click="link(item.sku_id,item.product_id)">
            <div class="list_detail">
              <p class="detail_title" @click="link(item.sku_id,item.product_id)">{{item.sku_name}}</p>
              <div class="spec_color">
                <p class="size" @mouseenter="attrshow(index)" @mouseleave="attrhidden()">
                  <span v-if="item.sku_attrs"><span v-for="(item1,index1) in JSON.parse(item.sku_attrs)" :key="index1">
                    <span v-if="index1<2">
                      {{item1.attr_name}}:<span style="color: #333;">{{item1.value.attr_value}}</span>;
                    </span>
                    <span v-if="index1>=2">...</span>
                  </span></span>
                </p>
                <span v-if="item.sku_attrs"><p class="sizevisible" v-if="JSON.parse(item.sku_attrs).length>=2" v-show="attrShowindex == index">
                  <span v-for="(item1,index2) in JSON.parse(item.sku_attrs)" :key="index2">
                    {{item1.attr_name}}:<span style="color: #333;">{{item1.value.attr_value}}</span>;
                  </span>
                </p></span>
                <p class="qty"><span>QTY:</span>{{item.goods_count}}</p>
              </div>
            </div>
            <div class="price_del">
              <i class="el-icon-error" @click="deleteItemCartOther(item.sku_id)"></i>
              <div class="price">$ {{item.totalPay.toFixed(2)}}</div>
            </div>
          </li>
        </ul>
        <div class="addMoreBtn" v-if="fullGiveList.length>0">赠品</div>
        <ul v-if="fullGiveList.length>0">
          <li style="margin-top:10px;" v-for="(item,index) in fullGiveList" :key="index">
            <img :src="item.sku_image" alt style="margin-right:0;" @click="link(item.sku_id,item.product_id)">
            <div class="list_detail">
              <p class="detail_title" @click="link(item.sku_id,item.product_id)">{{item.sku_name}}</p>
              <div class="spec_color">
                <p class="size" @mouseenter="attrshow(index)" @mouseleave="attrhidden()">
                  <span v-if="item.sku_attrs"><span v-for="(item1,index1) in JSON.parse(item.sku_attrs)" :key="index1">
                    <span v-if="index1<2">
                      {{item1.attr_name}}:<span style="color: #333;">{{item1.value.attr_value}}</span>;
                    </span>
                    <span v-if="index1>=2">...</span>
                  </span></span>
                </p>
                <span v-if="item.sku_attrs"><p class="sizevisible" v-if="JSON.parse(item.sku_attrs).length>=2" v-show="attrShowindex == index">
                  <span v-for="(item1,index2) in JSON.parse(item.sku_attrs)" :key="index2">
                    {{item1.attr_name}}:<span style="color: #333;">{{item1.value.attr_value}}</span>;
                  </span>
                </p></span>
                <p class="qty"><span>QTY:</span>1</p>
              </div>
            </div>
            <div class="price_del">
              <!-- <i class="el-icon-error"></i> -->
              <div class="price">$ {{item.activity_price}}</div>
            </div>
          </li>
        </ul>
        <div v-if="this.goodsList.length != 0 && goodsListOn.length == 0" class="empty_cart">
          <div class="noGoodsText">{{this.goodsNum}}items are unavailable, please check your shopping cart. </div>
          <div class="shopBtn" @click="goShopping()">Go Shopping ></div>
        </div>
        <div v-if="this.goodsList.length == 0" class="empty_cart">
          <div class="noGoodsText">The Shopping Cart is Empty!</div>
          <div class="shopBtn" @click="goShopping()">Go Shopping ></div>
        </div>
      </div>
      <div class="total" v-if="this.goodsList.length!=0">
        <div class="total_price">
          <span>Total Price:</span>
          <b>${{TotalPrice}}</b>
        </div>
        <div class="view_cart" @click="toShopCart()"><span>View Cart</span></div>
      </div>
    </div>
  </div>
</template>
<script>
import {getcartgoodscount,getGoodsList,userLogout,checkLogin} from "../api/register";
import Nav from "@/components/nav.vue"
import qs from 'qs'
import { mapGetters } from 'vuex';
  export default {
    components:{
      "nav-com":Nav
    },
    data() {
      return {
        keyword:'',//搜索关键字
        searchVal: '',
        searchInput: '',
        linkWordUrl: '',
        f_cate_id: '',
        s_cate_id: '',
        // titleColor: '#fff',
        titlePosition: 'start',
        showIconActive: 'el-icon-d-arrow-right',
        activityDetail: '',
        countDownList: '00天00时00分00秒',
        actEndTime: '2019-11-29 14:52:00',
        actCurrentTime: '2019-11-28 14:52:00',
        hList: [],
        hotList: [],
        historyList: [],
        login_status:true,//用户登录状态
        userName: 'Welcome',
        showHistory: false,
        titleActivity: true,// 活动头部显示
        TotalPrice:0,//购物车总价
        show:false,//购物车显示状态
        goodsList:[],//购物车列表
        goodsListOn:[],//上架商品列表
        goodsListOff:[],//已下架商品列表
        fullGiveList: [], // 赠品
        anotherGoodsList:[], //加购
        goodsNum: 0,//购物车商品数量
        activityShow: false,
        headerShow:false,//头部显示状态
        searchShow:false,//搜索框显示状态
        attrShowindex:-1,//缩略版购物车属性值显示状态
      };
    },
    mounted() {
        window.addEventListener("scroll", this.showIcon);
    },
    watch: {
      addCart(add){
        if(add == true){
          this.getGoodsCont()
        }
        this.$store.state.addCartState = false
      },
      showHistory: function (val, ol) {
        if (val) {
          var list = JSON.parse(localStorage.getItem('hList'))
          this.historyList = list.slice(0, 10)
          this.hotSearch()
        }
      },
      // $route(){
      //   this.s_cate_id = this.$route.query.s_cate_id
      //   this.f_cate_id = this.$route.query.f_cate_id
      // },
      // s_cate_id() {
      //   this.searchVal = ''
      // },
      // f_cate_id () {
      //   this.searchVal = ''
      // },
      searchVal: function (val, oV) {
        if (val) {
          this.showHistory = false
        } else {
          this.showHistory = true
        }
      },
      changeusername(user){
        if(user == true){
          this._checkLogin()
        }
        this.$store.state.username = false
      },
      'show': function(val, ov){
        if(val){
          this.getGoodsListFuc()
        }else{
          this.goodsListOn = []
          this.goodsListOff = []
        }
      }
    },
    created() {
      this._checkLogin()
      this.getGoodsCont()
      this.linkSearch()
      this.activityShowFnc()
      if (this.$route.query.keyword){
        this.searchVal = this.$route.query.keyword
      }
      // console.log('rrrrr', this.searchVal)
      if (!localStorage.getItem('hList')) {
        localStorage.setItem('hList', JSON.stringify(this.hList))
      }
    },
    computed: {
      ...mapGetters([
        'addCart',
        'changeusername'
      ]),
      Refresh: function () {
        var that = this
        if (this.$route.name === 'goodsList') {
          if (that.$store.state.keyWordFlag) {
            that.searchVal = that.$store.state.keyWord
            that.$store.state.keyWordFlag = false
          }
        } else {
          that.$store.state.keyWord = ''
          that.searchVal = that.$store.state.keyWord
          that.$store.state.keyWordFlag = false
        }
        return this.$store.state.keyWordFlag
      },
      showHistoryRefresh: function () {
        var that = this
        if (that.$store.state.searchShow === 1) {
          that.showHistory = false
          that.$store.state.searchShow = 0
        }
        return this.$store.state.searchFlag
      }
    },
    methods: {
      // 名称点击
      toActity (url) {
        window.location.href = url
      },
      // 活动详情
      showActivity(type) {
        if (type === 'el-icon-d-arrow-right') {
          this.showIconActive = 'el-icon-d-arrow-left'
          this.activityShow = true
        } else {
          this.showIconActive = 'el-icon-d-arrow-right'
          this.activityShow = false
        }
      },
      // timeFormat(param) {
      //   return param < 10 ? '0' + param : param;
      // },
      // 倒计时
      countDown() {
        // 获取当前时间
        let newTime = new Date(this.actCurrentTime).getTime();
        // 对结束时间
        let endTime = new Date(this.actEndTime).getTime();
        let time = (endTime - newTime)/1000;
        var interval = setInterval(() => {
          time --;
          if (time > 0) {
            // 获取天、时、分、秒
            let day = Math.floor(time/60/60/24)
            let hour = Math.floor((time / 3600) % 24)
            let min = Math.floor((time / 60) % 60)
            let sec = Math.floor(time % 60)
            day = day < 10 ? '0' + day : day
            hour = hour < 10 ? '0' + hour : hour
            min = min < 10 ? '0' + min : min
            sec = sec < 10 ? '0' + sec : sec
            // console.log('1111', day,hour,min,sec)
            this.countDownList = day + 'D ' + hour + 'h:' + min + 'm:' + sec + 's'
          } else { // 活动已结束，全部设置为'00'
            this.countDownList = '00天00时00分00秒'
            clearInterval(interval);
          }
        }, 1000);
      },
      inputFocus: function () {
        this.showHistory = true
      },
      closeHistory: function(){
        this.showHistory = false
      },
      // blurInput: function () {
      //   this.showHistory = false
      // },
      toSearch: function (str) {
        this.searchVal = str
        this.searchOver()
        this.showHistory = false
      },
      // 广告
      activityShowFnc: function () {
        var that = this
        this.$axios.post('api/topadvert/show',{}).then(res => {
          if (res.code === 200) {
          //  console.log('gggg', res)
            that.activityDetail = res.data.topadvert
            if (that.activityDetail) {
             that.titleActivity = true
              if (that.activityDetail.ad_end_time) {
                that.actEndTime = res.data.topadvert.ad_end_time
              }
              if (that.activityDetail.top_ad_location === 0) {
                that.titlePosition = 'flex-end'
              } else if (that.activityDetail.top_ad_location === 1) {
                that.titlePosition = 'center'
              } else if (that.activityDetail.top_ad_location === 2) {
                that.titlePosition = 'start'
              }
              if (that.activityDetail.countdown_status === 1) {
                if (this.actCurrentTime && that.actEndTime) {
                  that.countDown() // 倒计时
                }
              }
            } else {
             that.titleActivity = false
            }
            if (res.data.now) {
              that.actCurrentTime = res.data.now
            }
          }
        })
      },
      // 热词
      hotSearch: function () {
        this.$axios.post('api/show',{}).then(res => {
          if (res.code === 200) {
            this.hotList = JSON.parse(res.data)
          }
        })
      },
      // 搜索底纹
      linkSearch: function () {
        this.$axios.post('api/linkword/show',{}).then(res => {
          if (res.code === 200) {
            if (res.data) {
              this.searchInput = res.data.link_word_name
              this.linkWordUrl = res.data.url
            } else {
              this.searchInput = 'search for items or shops'
              this.linkWordUrl = ''
            }
          }
        })
      },
      // 历史记录
      clearHistory: function () {
        this.hList = []
        localStorage.setItem('hList', JSON.stringify(this.hList))
        var list = JSON.parse(localStorage.getItem('hList'))
        this.historyList = list.slice(0, 10)
      },
      // 搜索
      searchOver: function(e) {
        if (this.searchVal) {
          this.showHistory = false
          if (this.$route.name !== 'goodsList'){
            this.$router.push({
              path: '/goodsList',
              query: {
                keyword: this.searchVal
              }
            })
          } else {
            this.$store.state.searchVal = this.searchVal
            this.$store.state.searchFlag = true
            this.$router.push({
              path: '/goodsList',
              query: {
                keyword: this.searchVal
              }
            })
          }
          var hListNew = []
          var hListStr = localStorage.getItem('hList')
          var flag = false
          var k = 0
          var hListNewStr = JSON.parse(hListStr)
          hListNew = hListNewStr.slice(0, 10)
          if (hListNew.length > 0) {
            for(var i=0;i< hListNew.length; i++) {
              if(hListNew[i] === this.searchVal){
                flag = true
                k = i
                break
              }
            }
            if (flag) {
              hListNew.splice(k, 1)
              hListNew.unshift(this.searchVal)
            } else {
              hListNew.unshift(this.searchVal)
            }
          } else {
            hListNew.unshift(this.searchVal)
          }
          localStorage.setItem('hList', JSON.stringify(hListNew))
        } else {
          if (this.linkWordUrl){
            window.location.href = this.linkWordUrl
          }
        }
      },
      attrshow(num){
        this.attrShowindex = num
      },
      attrhidden(){
        this.attrShowindex = -1
      },
      //小头部搜索框显示
      searchIcon(){
        this.searchShow = !this.searchShow
      },
      // 头部切换
      showIcon() {
        if (!!document.documentElement.scrollTop && document.documentElement.scrollTop > 120) {
            this.headerShow = true;
            this.activityShow = false
            this.showIconActive = 'el-icon-d-arrow-right'
        } else {
            this.headerShow = false;
            this.searchShow = false;
        }
      },
      //购物车到商品详情
      link(skuid,spuid){
        this.$router.push('/goodsDetail/'+ spuid + '/'+ skuid)
      },
      //退出登录
      logout(){
        userLogout().then((res)=>{
          if(res.code === 200){
            this.$message({
              message: "success",
              type: "success"
            });
            localStorage.removeItem("userToken")
            this.userName = 'Login'
            this._checkLogin()
            this.getGoodsCont()
            this.$router.push('/')
          }
        })
      },
      //判断用户登录状态
      _checkLogin(){
        checkLogin().then((res)=>{
          if(res.code === '200' || res.code === 200){
            this.login_status = true;
            this.userName = res.data
          }else{
            this.login_status = false;
            this.userName = 'Login'
            localStorage.removeItem("userToken")
          }
        })
      },
      goShopping: function () {
        this.$router.push('/')
        this.show = false
      },
      visible:function(){
        this.show = true;
      },
      invisible:function(){
        this.show = false;
      },
      //获取购物车商品
      async getGoodsListFuc(){
        var goodsListOn = []
        var goodsListOff = []
        var total = 0
        let data = await getGoodsList();
        this.goodsList = data
        for (var i = 0;i<this.goodsList.length;i++){
          var obj = this.goodsList[i]
          if (this.goodsList[i].sku_status === 1) {
            goodsListOn.push(obj)
            for (var j = 0;j<goodsListOn.length;j++) {
              var itemPay = parseFloat(goodsListOn[j].activity_price?goodsListOn[j].activity_price:goodsListOn[j].sku_price) * goodsListOn[j].goods_count
              goodsListOn[j].totalPay = itemPay.toFixed(2)
            }
          } else if (this.goodsList[i].sku_status === 0){
            goodsListOff.push(obj)
          }
        }
        // for(var i = 0;i<goodsListOn.length;i++){
        //     total += Number(goodsListOn[i].totalPay)
        // }
        // this.TotalPrice = total.toFixed(2)
        this.goodsListOn = goodsListOn
        this.goodsListOff = goodsListOff
        this.getActivityGoodsList()
      },
      getActivityGoodsList () {
        var that = this
        that.$axios.post('api/getmyactivitycartsku', {}).then(res => {
          if(res instanceof Array){
            for (var i=0;i<res.length;i++) {
              that.$set(res[i],'totalPay', 0)
              if (res[i].activity_price) {
                res[i].totalPay = parseFloat(res[i].activity_price) * res[i].goods_count
              } else {
                res[i].totalPay = parseFloat(res[i].sku_price) * res[i].goods_count
              }
            }
            that.anotherGoodsList = res
          }
          console.log("*****22222", res)
          that.totalPayAdd()
        })
      },
      totalPayAdd () {
        var total = 0
        var total2 = 0
        for(var i = 0;i<this.goodsListOn.length;i++){
            total += Number(this.goodsListOn[i].totalPay)
        }
        for(var j = 0;j<this.anotherGoodsList.length;j++){
            total2 += Number(this.anotherGoodsList[j].totalPay)
        }
        this.TotalPrice = parseFloat(total + total2).toFixed(2)
        this.getActivityGoods(this.TotalPrice)
      },
      getActivityGoods (total) {
        var that = this
        var obj = qs.stringify({
          activity_type: 3,
          subtotal: total
        })
        that.$axios.post('api/cartactivityitembysubtotal', obj).then(res => {
          if (res.length > 0) {
            console.log('jjjjjjj', res)
            that.fullGiveList = res
          }
        })
      },
      //删除购物车商品
      delList(skuId) {
        this.$axios.post('api/deltocart/' + skuId).then(res => {
          this.getGoodsListFuc()
          this.getGoodsCont()
          this.$store.state.delcartList = true
          // if (res.status === 200) {
          //   this.getGoodsListFuc()
          //   this.getGoodsCont()
          // }
        })
      },
      // 删除加购商品
      deleteItemCartOther(id){
        var that = this
        // that.btnLoading = true
        var obj = qs.stringify({
          sku_id: id
        })
        that.$axios.post('api/deltoactivitycart', obj).then(res => {
          this.getGoodsListFuc()
          this.getGoodsCont()
          this.$store.state.delcartList = true
          // that.$store.state.addCartState = true
        })
      },
      //获取购物车商品总量
      async getGoodsCont(){
        let data = await getcartgoodscount()
        this.goodsNum = data
      },
      handleGoHome(){
        this.$router.push({
          path: '/'
        })
      },
      toShopCart: function(){
        this.$router.push('/shoppingCar')
        this.show = false;
      },
      HandleLogin(){
        this.$router.push({
          path: '/login'
        })
      },
    }
  };
</script>
<style lang="scss" scoped>
    @import "@/assets/css/header.scss"
</style>
