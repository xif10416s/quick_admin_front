<template>
  <div class="footer">
    <div class='data'>
      <img src="~@/assets/wxslogo.png" width="10%"/>
      <p>江苏省无锡市锡山区安镇街道东翔路578号红豆财富广场1-2层、5-11层</p>
    </div>
  </div>
</template>

<script>
  export default {
    name: "LayoutFooter",
    src: '../../assets/footer_logo.png'
  }
</script>

<style lang="less" scoped>
  .footer {
    padding: 0 16px;
    margin: 48px 0 24px;
    text-align: center;

    .links {
      margin-bottom: 8px;

      a {
        color: rgba(0, 0, 0, .45);

        &:hover {
          color: rgba(0, 0, 0, .65);
        }

        &:not(:last-child) {
          margin-right: 40px;
        }
      }
    }
    .copyright {
      color: rgba(0, 0, 0, .45);
      font-size: 14px;
    }
  }
</style>