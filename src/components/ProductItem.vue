<template>
  <div class="menu-product">
    <div class="clearfix m-pro-item">
      <div class="fl pro-img">
        <img class="auto-img" :src="item.smallImg" alt="" />
      </div>
      <div class="fl text">
        <div class="pro-text">
          <div class="pro-name">{{ item.name }}</div>
          <div class="pro-enname">{{ item.enname }}</div>
        </div>
      </div>
      <div class="fl price">￥{{ item.price }}</div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ProductItem',
  props: {
    item: {
      type: Object,
      default() {
        return {}
      }
    }
  }
}
</script>

<style lang="less" scoped>
.menu-product {
  padding: 10px;
}

.m-pro-item {
  padding: 10px;
  border-radius: 8px;
  background-color: #fff;
  margin-bottom: 10px;
  &:last-child {
    margin-bottom: 0;
  }
}

.pro-img {
  width: 70px;
  height: 70px;
  background-color: #ddd;
  border-radius: 8px;
  overflow: hidden;
}

.text {
  width: calc(~"100% - 150px");
  margin: 0 10px;
}

.price {
  width: 50px;
  line-height: 70px;
  color: #006699;
  font-weight: bold;
  font-size: 14px;
  text-align: right;
}

.pro-text {
  margin-top: calc(35px / 2);
}

.pro-name {
  font-size: 14px;
}

.pro-enname {
  font-size: 12px;
  margin-top: 5px;
}
</style>