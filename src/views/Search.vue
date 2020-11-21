<template>
  <div class="search">
    <van-nav-bar left-text="返回" left-arrow fixed @click-left="back">
      <template #right>
        <div class="home-search">
          <van-search
            v-model="name"
            ref="search"
            show-action
            @search="search"
            placeholder="输入商品名称"
          />
        </div>
      </template>
    </van-nav-bar>

     <BgBox>
      <div v-if="products.length > 0">
        <div v-for="(item, index) in products" :key="index" @click="goDetail(item.pid)">
          <ProductItem :item="item"></ProductItem>
        </div>
      </div>
      <van-empty description="暂无搜索结果" v-else/>
    </BgBox>

  </div>
</template>

<script>
import BgBox from "../components/BgBox.vue";
import ProductItem from "../components/ProductItem.vue";
export default {
  name: "Search",

  components: {
    BgBox,
    ProductItem,
  },

  data() {
    return {
      //搜索商品关键字
      name: "",

      //搜索商品数据
      products: [],
    };
  },

  created() {
    this.$nextTick(() => {
      console.log(this.$refs);
      //获取搜索框
      let searchIpt = this.$refs.search.querySelector('[type="search"]');
      console.log("searchIpt ==> ", searchIpt);

      //获取焦点
      searchIpt.focus();
    });
  },

  methods: {
    back() {
      this.$router.go(-1);
    },

    //搜索
    search() {
      this.$toast.loading({
        message: "加载中...",
        forbidClick: true,
        duration: 0,
      });

      this.axios({
        method: "GET",
        url: "/search",
        params: {
          appkey: this.appkey,
          name: this.name,
        },
      })
        .then((result) => {
          this.$toast.clear();
          console.log("search result ==> ", result);

          if (result.data.code == "Q001") {
            this.products = result.data.result;
          } else {
            this.$toast(result.data.msg);
          }
        })
        .catch((err) => {
          this.$toast.clear();
          console.log("err ==> ", err);
        });
    },

     //查看商品详情
    goDetail(pid) {
      console.log('pid ==> ', pid);
      this.$router.push({name: 'Detail', params: {pid}});
    },
  },
};
</script>

<style lang="less" scoped>
.search {
  padding-top: 46px;

  /deep/ .van-nav-bar .van-icon {
    color: #006699;
  }

  /deep/ .van-nav-bar__text {
    color: #006699;
  }

  /deep/ .van-nav-bar__right {
    width: calc(~"100% - 110px");
  }

  /deep/ .home-search {
    width: 100%;
  }
  /deep/ .home-search .van-search {
    padding: 0;
    border-radius: 17px;
    overflow: hidden;
  }

  /deep/ .home-search .van-icon {
    color: #006699;
  }
}
</style>