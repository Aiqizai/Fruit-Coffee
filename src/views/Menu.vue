<template>
  <div class="menu">
    <div class="menu-nav">
      <div class="menu-search">
        <van-search placeholder="输入商品名称" @focus="searchFocus" />
      </div>
    </div>
    <div class="product-wrapper clearfix">
      <!-- 商品类型tab栏 -->
      <div class="menu-option fl">
        <div
          class="menu-item"
          v-for="(item, index) in menuOptions"
          :key="index"
          @click="toggleMenu(index, item.type)"
        >
          <div class="m-item">
            <div class="m-icon">
              <img
                class="auto-img"
                :src="menuIndex == index ? item.activeIcon : item.inactiveIcon"
                alt=""
              />
            </div>
            <div class="m-text" :class="{ active: menuIndex == index }">
              {{ item.title }}
            </div>
          </div>
        </div>
      </div>
      <!-- 商品列表 -->
      <div class="product-list fr">
        <div
          v-for="(item, index) in productData"
          :key="index"
          @click="goDetail(item.pid)"
        >
          <ProductItem :item="item"></ProductItem>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import "../assets/less/menu.less";
import ProductItem from "../components/ProductItem.vue";

export default {
  name: "Menu",

  data() {
    return {
      menuIndex: 0,

      menuOptions: [
        {
          title: "推荐",
          inactiveIcon: require("../assets/images/icons_10.png"),
          activeIcon: require("../assets/images/icons_09.png"),
        },
        {
          title: "拿铁",
          inactiveIcon: require("../assets/images/icons_11.png"),
          activeIcon: require("../assets/images/icons_12.png"),
        },
        {
          title: "咖啡",
          inactiveIcon: require("../assets/images/icons_14.png"),
          activeIcon: require("../assets/images/icons_13.png"),
        },
        {
          title: "瑞纳冰",
          inactiveIcon: require("../assets/images/icons_15.png"),
          activeIcon: require("../assets/images/icons_16.png"),
        },
      ],

      //商品数据
      productData: [],

      // 
      recordProductData: []
    };
  },

  created() {
    //获取商品类型
    this.getTypes();
  },

  methods: {
    //切换菜单商品
    toggleMenu(index, type) {
      if (this.menuIndex == index) {
        return;
      }

      this.menuIndex = index;

      // this.recordProductData = JSON.parse(window.localStorage.getItem("productData"));

      // if(this.recordProductData) {
      //    this.recordProductData.map((item,index) => {

      //    })
      // }

      this.getProductByType(type);
    },

    //获取商品类型
    getTypes() {

      this.$toast.loading({
        message: "加载中...",
        forbidClick: true,
        duration: 0,
      });

      this.axios({
        method: "GET",
        url: "/type",
        params: {
          appkey: this.appkey
        },
      })
        .then((result) => {
          this.$toast.clear();
          console.log("updateDesc result ==> ", result);

          if (result.data.code == 400) {
            let data = result.data.result;
            data.unshift({
              type: 'isHot',
              typeDesc: '推荐'
            })

            this.menuOptions.map(v => {
              for (let i = 0; i < data.length; i++) {
                if (v.title == data[i].typeDesc) {
                  v.type = data[i].type;
                  break;
                }
              }
            })

            let type = this.menuOptions[this.menuIndex].type;

            this.getProductByType(type);

          }

          // console.log('this.menuOptions ==> ', this.menuOptions)

        })
        .catch((err) => {
          this.$toast.clear();
          console.log("err ==> ", err);
        });
    },

    //根据类型获取商品
    getProductByType(type) {

      let params = {
        appkey: this.appkey
      };
      if (type == 'isHot') {
        params.key = 'isHot';
        params.value = 1;
      } else {
        params.key = 'type';
        params.value = type;
      }

      console.log('params ==> ', params);

      this.$toast.loading({
        message: "加载中...",
        forbidClick: true,
        duration: 0,
      });

      this.axios({
        method: "GET",
        url: "/typeProducts",
        params,
      })
        .then((result) => {
          this.$toast.clear();
          console.log("getProductByType result ==> ", result);

          if (result.data.code == 500) {
            this.productData = result.data.result;
            // localStorage.setItem("productData",JSON.stringify(this.productData))
            // let storage = window.localStorage.getItem("productData");
            // storage = storage ? JSON.parse(storage) : [];
            // storage.push({item: this.productData});
            // localStorage.setItem("productData", JSON.stringify(storage));
          }

        })
        .catch((err) => {
          this.$toast.clear();
          console.log("err ==> ", err);
        });
    },

    //查看商品详情
    goDetail(pid) {
      this.$router.push({ name: 'Detail', params: { pid } });
    },

    //搜索栏获取焦点
    searchFocus() {
      this.$router.push({ name: 'Search' });
    }
  },

  components: {
    ProductItem
  }
};
</script>