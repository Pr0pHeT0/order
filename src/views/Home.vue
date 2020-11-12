<template>
  <div>
    <!-- <a @click="$router.push('/order')">aaaaaaaa</a> -->
    <div class="login-warp">
      <div class="content">
        <div class="input-row">
          <button
            type="button"
            @click="show = true"
            class="input-col input-btn"
          >
            用餐人数
          </button>
        </div>
        <div class="input-row">
          <button
            type="button"
            @click="$router.push('/order')"
            class="input-col input-btn"
          >
            开始点餐
          </button>
        </div>
      </div>
    </div>
    <van-popup v-model="show" position="bottom" :style="{ height: '40%' }"
      ><van-picker
        title="用餐人数"
        show-toolbar
        :columns="columns"
        @change="onChange"
        @cancel="show=false"
        @confirm="show=false"
    /></van-popup>
  </div>
</template>

<script>
export default {
  data() {
    return {
      columns: ["1人", "2人", "3人", "4人", "5人", "6人", "7人", "8人"],
      show: false,
    };
  },
  methods:{
    onChange(picker, value, index){
      this.$store.state.cnt=index+1
    }
  }
};
</script>

<style lang="less">
.login-warp {
  position: absolute;
  width: 100%;
  height: 100%;
  background: #fff;
  .content {
    padding-top: 50%;
  }
  .logo-title {
    max-width: 35%;
    margin-bottom: 60px;
    display: block;
    margin: auto;
    margin-bottom: 1.333333rem;
  }
  .input-row {
    height: 1.6rem;
    color: white;
    width: 80%;
    margin: auto;
    margin-top: 3rem;
  }
  .input-col {
    width: 100%;
    height: 3rem;
    border: none;
    outline: none;
    border-radius: 0.586667rem;
    background: #f7f7f7;
    text-indent: 1.2rem;
    color: #666;
  }
  .input-btn {
    background-color: #366aff;
    text-indent: 0;
    font-size: 18px;
    color: #fff;
  }
}
</style>

