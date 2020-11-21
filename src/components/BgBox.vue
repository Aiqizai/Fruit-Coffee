<template>
  <div class="bg-box">
    <div class="bg"></div>
    <div class="bg-content">
      <slot></slot>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'BgBox'
  }
</script>

<style lang="less" scoped>
.bg-box{

  .bg{
    height: 100px;
    background-color: #006699;
    border-bottom-left-radius: 10px;
    border-bottom-right-radius: 10px;
  }

  .bg-content{
    margin: 0 10px;
    position: relative;
    top: -25px;
    background-color: #fff;
    border-radius: 8px;
    padding: 10px;
  }
}
</style>