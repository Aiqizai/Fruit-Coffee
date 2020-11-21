<template>
  <div class="home">
    <van-nav-bar fixed>
      <template #left>
        <div class="home-nav">
          <div class="t1">下午好</div>
          <div class="t2">{{userInfo.nickName}}</div>
        </div>
      </template>
      <template #right>
        <div class="home-search">
          <van-search placeholder="输入商品名称" @focus="searchFocus"/>
        </div>
      </template>
    </van-nav-bar>

    <div class="home-content">
      <!-- 轮播图 -->
      <div class="banner-box">
        <van-swipe @change="changeCurrentIndex">
          <!-- :autoplay="2000" -->
          <van-swipe-item v-for="item in bannerData" :key="item.pid">
            <img
              class="auto-img"
              :src="item.bannerImg"
              alt=""
              @click="goDetail(item.pid)"
            />
            <div class="pro-bannername">{{item.name}}</div>
          </van-swipe-item>

          <template #indicator>
            <div class="index-box">
              <div
                :class="{ active: index == currentIndex }"
                v-for="(item, index) in bannerData"
                :key="index"
                :style="{left: left + 'px'}"
              ></div>
            </div>
          </template>
        </van-swipe>
      </div>
      <!-- 自提/配送 -->
      <div class="deliverySelf-box">
        <div class="self-box clearfix">
          <div class="imgBox fl">
            <img class="auto-img" src="../assets/images/self.png" alt="" />
          </div>
          <div class="fl">
            <div class="text1">自提</div>
            <div class="text2">在线点，到点取</div>
          </div>
        </div>
        <div class="delivery-box">
          <div class="imgBox fl">
            <img class="auto-img" src="../assets/images/delivery.png" alt="" />
          </div>
          <div class="fl">
            <div class="text1">外送</div>
            <div class="text2">30分钟必达</div>
          </div>
        </div>
      </div>
      <!-- 商品 -->
      <div class="product-box">
        <div>
          <div class="clearfix pro-title-box">
            <span class="pro-title">热卖推荐</span>
          </div>
          <div class="products clearfix">
            <div
              class="pro-item fl"
              v-for="item in HotProducts"
              :key="item.id"
              @click="goDetail(item.pid)"
            >
              <div>
                <img class="auto-img" :src="item.smallImg" />
                <!-- 热卖图片 -->
                <!-- <div class="hot-img">
                  <img class="auto-img" src="../assets/images/hot.png" alt="">
                </div> -->
                <!-- hot标签 -->
                <div class="hot">hot</div>
              </div>
              <div class="pro-info">
                <div class="pro-name one-text">{{ item.name }}</div>
                <div class="pro-enname one-text">{{ item.enname }}</div>
                <div class="pro-price">￥{{ item.price }}</div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import '../assets/less/home.less';

export default {
  name: 'Home',
  data() {
    return {
      // 轮播索引
      currentIndex: 0,
      // 轮播数据
      bannerData: [],
      // 热卖商品数据
      HotProducts: [],
      //用户信息
      userInfo: {},
      // 轮播索引高亮的位置
      left: 0
    };
  },
  created() {
    this.getBannerData()
    this.getHotProducts()
    //查询用户信息
    this.getUserInfo();
  },
  methods: {
    // 修改轮播图片索引
    changeCurrentIndex(index) {
      // console.log('index ==> ', index);
      this.currentIndex = index;
      this.left = (index * 25) >= 100 ? 0 : this.left = (index * 25);
    }, 
    // 获取轮播图数据
    getBannerData() {
      this.$toast.loading({
        message: '加载中...',
        forbidClick: true,
        duration: 0
      });

      this.axios({
        //请求类型
        method: 'GET',
        //请求路径
        url: '/banner',

        params: {
          appkey: this.appkey
        }

      }).then(result => {
        this.$toast.clear();


        // console.log('result ==> ', result);

        if (result.data.code == 300) {
          this.bannerData = result.data.result;
        }

      }).catch(err => {
        this.$toast.clear();

        console.log('err ==> ', err);
      })

    },
    // 获取热卖商品
    getHotProducts() {
      this.$toast.loading({
        message: '加载中...',
        forbidClick: true,
        duration: 0
      });

      this.axios({
        //请求类型
        method: 'GET',
        //请求路径
        url: '/typeProducts',

        params: {
          appkey: this.appkey,
          key: 'isHot',
          value: 1
        }

      }).then(result => {
        this.$toast.clear();


        console.log('result ==> ', result);

        if (result.data.code == 500) {
          this.HotProducts = result.data.result;
        }

      }).catch(err => {
        this.$toast.clear();

        console.log('err ==> ', err);
      })

    },
    // 查看商品详情页面
    goDetail(pid) {
      this.$router.push({ name: 'Detail', params: { pid } });
    },
    //获取用户信息
    getUserInfo() {
      let tokenString = localStorage.getItem("__tk");

      if (!tokenString) {
        return;
      }

      this.$toast.loading({
        message: "加载中...",
        forbidClick: true,
        duration: 0,
      });

      this.axios({
        method: "GET",
        url: "/findMy",
        params: {
          appkey: this.appkey,
          tokenString
        },
      })
        .then((result) => {
          this.$toast.clear();
          console.log("getUserInfo result ==> ", result);
          if (result.data.code == 'A001') {
            this.userInfo = result.data.result[0];
          }

        })
        .catch((err) => {
          this.$toast.clear();
          console.log("err ==> ", err);
        });
    },

    //搜索栏获取焦点
    searchFocus() {
      this.$router.push({ name: 'Search' });
    }
  }
}
</script>