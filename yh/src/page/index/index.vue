<template>
  <div id="container">
    <header class="header">
    	  <div class="back">
            <i class="iconfont">&#xe624;</i>
        </div>
      	<div class="search">
          <i class="iconfont">&#xe634;</i>
          <span>输入城市/景点/游玩主题</span>
        </div>
      	<div class="city">
          <span>城市</span>
          <i class="iconfont">&#xe60b;</i>
        </div>
    </header>	

    <swiper :options="swiperOption">
      <swiper-slide v-for="item in swiperInfo" :key="item.id">
        <div class="swiper-img-con">
          <img  class="swiper-img" :src="item.imgUrl"/>
        </div>
      </swiper-slide>
      <div class="swiper-pagination"  slot="pagination"></div>
    </swiper>

    <swiper :options="swiperList" class="classify">
      <swiper-slide v-for="(pageInfo, index) in pages" :key="index">
        <div class="icon-wrapper">
          <div v-for="item in pageInfo" :key="item.id" class="icon-item">
            <div class="icon-img-con">
              <img class="icon-img" :src="item.imgUrl"/>
              <span>{{item.name}}</span>
            </div>
          </div>
        </div>
      </swiper-slide>
      <div class="swiper-pagination"  slot="pagination"></div>
    </swiper> 

    <div class="positionList">
      <ul>
        <li class="position">
          <i class="iconfont">&#xe611;</i>
          <a href="#">定位失败</a>
        </li>
        <li>
          <i class="iconfont">&#xe6a4;</i>
          <a href="#">5折泡温泉</a>
        </li>
      </ul>
    </div>

    <div class="hot">
      <ul>
        <li v-for="item in hotSpring" :key="item.id" class="hotSpring">
          <img :src="item.imgUrl"/>
        </li>
        
      </ul>
    </div>

    <div class="rotRecommend">热销推荐</div>

    <div class="travelList">
      <dl v-for="item in travelSite" :key="item.id">
        <dt>
          <img :src="item.imgUrl" />
        </dt>
        <dd class="address">{{item.address}}</dd>
        <dd class="site">{{item.site}}</dd>
        <dd class="price">
          <span>¥</span>
          <span>{{item.price}}</span>
          <span>起</span>
        </dd>
      </dl>
      
    </div>
    <div class="examine">查看所有产品</div>

    <div class="weekend">周末去哪儿</div>

    <div class="scenicSpot">
      <dl v-for="item in scenicSpot" :key="item.id">
        <dt>
          <img :src="item.imgUrl" />
        </dt>
        <dd>{{item.txt1}}</dd>
        <dd>{{item.txt2}}</dd>
      </dl>
    </div>
    <div class="ticketPrice">
      <i class="iconfont">&#xe611;</i>
      <b>票面价</b>
      <span>是指通过景区指定窗口售卖的纸质门票上标注的价格</span>
    </div>

    <footer class="footer">
      <ul class="footer_t">
        <li>
          <i class="iconfont">&#xe611;</i>
          <a href="#">机票</a>
        </li>
        <li>
          <i class="iconfont">&#xe611;</i>
          <a href="#">酒店</a>
        </li>
        <li>
          <i class="iconfont">&#xe611;</i>
          <a href="#">公寓</a>
        </li>
        <li>
          <span>|</span>
          <a href="#">更多</a>
        </li>
      </ul>
      <ul class="footer_b">
        <li>
          <a href="#">登录</a>
        </li>
        <li>
          <a href="#">我的订单</a>
        </li>
        <li>
          <a href="#">最近浏览</a>
        </li>
        <li>
          <a href="#">关于我们</a>
        </li>
      </ul>
    </footer>
  </div>

  
  
</template>

<script>
export default {
  name: 'Index',
  data () {
    return {
      swiperInfo: [],
      iconInfo: [],
      hotSpring: [],
      travelSite: [],
      scenicSpot: [],
      swiperOption: {
        autoplay: 1000,
        loop: true,
        pagination: '.swiper-pagination'
      },
      swiperList: {
        pagination: '.swiper-pagination'
      }
    }
  },

  computed: {
    pages () {
      const pages = []
      this.iconInfo.forEach((value, index) => {
        let page = Math.floor(index / 8)
        if (!pages[page]) {
          pages[page] = []
        }
        pages[page].push(value)
      })
      return pages
    }
  },

  methods: {
    getIndexData () {
      this.$http.get('/static/index.json').then(this.handleGetDataSucc.bind(this))
    },

    handleGetDataSucc (res) {
      const body = res.body
      if (body && body.data && body.data.swiper) {
        this.swiperInfo = body.data.swiper
        this.iconInfo = body.data.icons
        this.hotSpring = body.data.hotSpring
        this.travelSite = body.data.travel
        this.scenicSpot = body.data.scenicSpot
      }
    }
  },

  created () {
    this.getIndexData()
  }
}
</script>


<style scoped>
  #container {
    background: #f5f5f5;
  }
	.header {
    display: flex;
    background: #05bad5;
    color: #fff;
  }
  .back {
    width: .64rem;
    line-height: .86rem;
    text-align: center;
  }
  .search {
    flex: 1;
    margin: .14rem .18rem;
    background: #fff;
    border-radius: .1rem;
    line-height: .6rem;
  }
  .search>i {
    color: #e4e7ea;
    padding: 0 .04rem;
  }
  .search>span {
    color: #e4e7ea;
  }
  .city {
    
    line-height: .86rem;
    text-align: center;
  }
  .city>span {
    float:left;
  }
  .city>i{
    float:left;
    font-size: .5rem;
    margin-left: .04rem;
    margin-left: -0.1rem;
  }
  .swiper-img-con {
    overflow: hidden;
    width: 100%;
    height: 0;
    padding-bottom: 31.25%;
  }
  .swiper-img {
    width: 100%;
  }
  .classify {
    background: #fff;
  }
  .classify {
    height: 3.8rem;
  }
  .icon-item {
    box-sizing: border-box;
    float: left;
    width: 25%;
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
  }
  .icon-img-con {
    height: 1.32rem;
    display: flex;
    justify-content: center;
    flex-direction: column;
    justify-content: space-between;
    align-items: center;
    margin-top: .23rem;
  }
  .icon-img {
    width: .64rem;
  }
  .positionList {
    height: .98rem;
    border-top: 1px solid #e1e1e1;
    background: #fff;
  }
  .positionList ul li {
    float: left;
    width: 49.8%;
    height:.98rem;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .positionList ul li a {
    color: #1e1e1e;
    margin-left: .03rem;
  }
  .position{
    border-right: 1px solid #e1e1e1;
  }
  .hot {
    padding-top: .2rem;
  }
  .hot ul {
    height: 1.4rem;
  }
  .hotSpring {
    width: 49.8%;
    float: left;
    border-top: 1px solid #e1e1e1;
    border-bottom: 1px solid #e1e1e1;
    background: #fff;
  }
  .hotSpring:nth-of-type(1) {
    border-right: 1px solid #e1e1e1;
  }
  .hotSpring img {
    width: 100%;
    height: 1.4rem;
  }
  .rotRecommend, .weekend {
    height: .8rem;
    line-height: .8rem;
    text-indent: .27rem; 
  }
  .travelList dl {
    width: 100%;
    height: 1.98rem;
    border-bottom: 1px solid #e1e1e1;
    background: #fff;
  }
  .travelList dl dt img {
    width: 1.4rem;
    padding: .22rem;
  }
  .travelList dl dt, .travelList dl dd {
    float: left;
  }
  .travelList dl dd {
    width: 5.4rem;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    margin-top: .2rem;
  }
  .address {
    margin-top: .22rem;
    color: #131313;
    font-size: .26rem;
  }
  .site {
    color: #9c9c9c;
    font-size: .25rem;
    line-height: .28rem;
  }
  .price span:nth-of-type(1) {
    color: #e39c44;
    font-size: .17rem;
  }
  .price span:nth-of-type(2) {
    color: #e39c44;
    font-size: .25rem;
  }
  .price span:nth-of-type(3) {
    color: #919191;
    font-size: .21rem;
  }
  .examine {
    height: .88rem;
    line-height: .88rem;
    font-size: .24rem;
    text-align: center;
    color: #58a5bf;
    background: #fff;
  }
  .scenicSpot {
    background: #f6f6f6;
  }
  .scenicSpot dl {
    margin-top: .1rem;
    height: 4.07rem;
    background: #fff;
  }
  .scenicSpot dl dt {
    width: 100%;
  }
  .scenicSpot dl dt img {
    width: 100%;
  }
  .scenicSpot dl dd {
    padding-left: .21rem;
    width: 80%;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }
  .scenicSpot dl dd:nth-of-type(1) {
    font-size: .25rem;
    color: #1c1c1c;
    margin-top: .25rem;
    margin-bottom: .18rem;
  }
  .scenicSpot dl dd:nth-of-type(2) {
    font-size: .23rem;
    color: #5d5d5d;
    margin-bottom: .32rem;
  }
  .ticketPrice {
    margin-top: .1rem;
    height: .6rem;
    background: #fff;
    line-height: .6rem;
  }
  .ticketPrice i {
    padding-left: 4px;
  }
  .ticketPrice span {
    font-size: 12px;
  }
  .footer_t {
    height: .7rem;
    line-height: .7rem;
    display: flex;
    justify-content: center;
  }
  .footer_t li {
    float: left;
    width:20%;
  }
  .footer_t li a {
    color: #acacac;
  }
  .footer_b {
    background: #f5f5f5;
    height: .39rem;
  }
  .footer_b li {
    float: left;
    width:20%;
    text-align: center;
  }
</style>
