<template>
  <div>
    <Card shadow title="社区">
      <row class="join-page" :gutter="32">
        <i-col span="10">
          <!-- <img class="qq-group-img" src="../assets/images/icon-qr-qq-wechat.jpg" /> -->
        </i-col>
        <i-col span="14">
          <div class="join-page-other">
            更多内容，敬请期待~
          </div>
        </i-col>
      </row>
    </Card>
  </div>
</template>

<script>
export default {
  name: 'join_page',
  data () {
    return {}
  }
}
</script>
<style>
.join-page {
  text-align: center;
}
.qq-group-img {
  width: 100%;
}
.join-page-other-icon {
  width: 20px;
  vertical-align: middle;
  margin-right: 6px;
}
.join-page-other {
  text-align: left;
}
.join-page-other .ivu-btn {
  margin-right: 6px;
}
</style>
