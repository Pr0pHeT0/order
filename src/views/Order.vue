<template>
  <div>
    <van-nav-bar
      title="点餐"
      left-text="返回"
      left-arrow
      @click-left="$router.push('/')"
    />
    <van-row type="flex" style="height: calc(100vh - 46px)">
      <van-sidebar v-model="activeMenu" type="flex">
        <van-sidebar-item title="特色菜" />
        <van-sidebar-item title="荤菜" />
        <van-sidebar-item title="素菜" />
        <van-sidebar-item title="汤" />
        <van-sidebar-item title="食材" />
      </van-sidebar>
      <div
        style="
          width: calc(100vw - 80px);
          height: calc(100vh - 46px);
          overflow: scroll;
        "
        v-if="activeMenu != 4"
      >
        <template v-for="(food, index) in foodList">
          <van-card
            :key="food.name"
            :num="food.quantity"
            :price="food.price"
            :desc="food.ingredient"
            :title="food.name"
            :thumb="food.pic"
            @click="showDetail(index)"
          >
          </van-card>
        </template>
      </div>
      <div
        v-if="activeMenu == 4"
        style="
          width: calc(100vw - 80px);
          height: calc(100vh - 46px);
          overflow: scroll;
        "
      >
        <template v-for="(material,index) in materialList">
          <van-card
            :key="material.name"
            :desc="material.desc"
            :time="material.time"
            :loc="material.loc"
            :title="material.name"
            :thumb="material.pic"
            @click="showMaterialdetail(index)"
          >
          </van-card>
        </template>
      </div>
    </van-row>



     <!-- 菜品详情 -->
    <van-popup
      v-model="showPopup"
      round
      position="bottom"
      :style="{ height: '55%' }"
      ><div style="padding-top: 28px; padding-left: 10px">
        <van-row>
          <van-col span="8">
            <img
              style="padding-left: 12px"
              width="90px"
              height="80px"
              :src="foodDetail.pic"
            />
          </van-col>
          <van-col span="16"
            >{{ foodDetail.name }}<br /><br />数量：<van-stepper
              style="position: fixed; right: 50px"
              min="0"
              v-model="foodDetail.quantity"
              @change="sum"
          /></van-col>
        </van-row>
        <van-divider />
        <div style="height: 24vh; overflow-y: scroll">
          菜品详情：
          {{ foodDetail.detail }}
          <br /><br />
          食材/原料：{{ foodDetail.ingredient }} <br /><br />
          适用人群：{{ foodDetail.audience }}
        </div>
        <van-submit-bar
          button-text="加菜单"
          @submit="
            foodDetail.quantity += 1;
            showPopup = false;
          "
        />
      </div>
    </van-popup>



    <!-- 食材详情 -->
    <van-popup
      v-model="showPopup_2"
      round
      position="bottom"
      :style="{ height: '70%' }"
      ><div style="padding-top: 28px; padding-left: 10px">
        <van-row>
          <van-col span="24">
            <img
              style="margin:20px"
              width="250px"
              height="200px"
              :src="materialDetail.pic"
            />
          </van-col>
        </van-row>
        <van-divider />
        <div style="height: 24vh; overflow-y: scroll">
          食材介绍: {{ materialDetail.desc }}
          <br /><br />
          产地: {{ materialDetail.loc }}
          <br /><br />
          发货时间: {{ materialDetail.time }}
          <br /><br />
          到货时间: {{ materialDetail.time }}
        </div>
      </div>
    </van-popup>
    <div
      style="
        position: fixed;
        width: 100%;
        bottom: 0px;
        height: 50px;
        background-color: white;
        z-index: 1;
        box-shadow: 0px -2px 5px grey;
      "
      @click="showCart"
    >
      <van-icon
        size="2em"
        style="margin: 10px"
        name="shopping-cart"
        :badge="foodSum"
      ></van-icon>
      <p style="position: fixed; right: 35px; bottom: 8px; margin: 8px">
        合计：{{ priceSum }}元
      </p>
    </div>
    <van-popup
      v-model="showCartpopup"
      position="bottom"
      :style="{ height: '35%' }"
    >
      <div style="margin: 20px">
        <div
          style="width: 100%; text-align: center; color: grey"
          v-if="cartList == ''"
        >
          购物车为空
        </div>
        <template v-for="food in cartList">
          <van-cell
            :key="food.name"
            :title="food.name"
            :value="
              'x' + food.quantity + '    ' + food.quantity * food.price + '元'
            "
          ></van-cell>
        </template>
        <div v-if="cartList != ''">
          <p style="position: fixed; right: 125px; bottom: 8px; margin: 8px">
            合计：{{ priceSum }}元
          </p>
          <van-button
            @click="$dialog.alert({ message: '提交成功' })"
            round
            small
            style="
              height: 30px;
              position: fixed;
              right: 20px;
              bottom: 4px;
              margin: 8px;
            "
            color="linear-gradient(to right, #ff6034, #ee0a24)"
            >提交订单</van-button
          >
        </div>
      </div>
    </van-popup>
  </div>
</template>

<style >
</style>

<script>
export default {
  data() {
    return {
      customerCnt: 3,
      showCartpopup: false,
      foodSum: 0,
      priceSum: 0,
      activeMenu: 0,
      showPopup: false,
      showPopup_2: false,
      materialList: [
        { 
          name: "鸡肉", 
          desc: "肉质细嫩，滋味鲜美", 
          time: "2020年11月11日11点11分", 
          loc: "无锡", 
          pic: "./ji.jpg" },
        {
          name: "牛肉",
          desc: "蛋白质含量高，脂肪含量低，味道鲜美",
          time: "2020年11月11日11点11分",
          loc: "内蒙古",
          pic: "./niurou.jpg",
        },
        { 
          name: "西红柿", 
          desc: "果实营养丰富，具特殊风味", 
          time: "2020年11月11日11点11分", 
          loc: "苏州",
          pic: "./xi.jpg" },
      ],
      foodList: [
        {
          name: "红烧肉",
          detail:
            "其以五花肉为制作主料，最好选用肥瘦相间的三层肉（五花肉）来做，锅具以砂锅为主，做出来的肉肥瘦相间，香甜松软，营养丰富，入口即化。",
          quantity: 0,
          audience: "食肉人士",
          price: 19.9,
          ingredient: "猪肉",
          pic: "./hong.jpg",
        },
        {
          name: "回锅肉",
          detail:
            "是一种四川传统菜式中家常（味型）菜肴的代表菜肴之一，属于川菜系列。制作原料主要有猪后臀肉、青椒、蒜苗等，口味独特，色泽红亮，肥而不腻。",
          quantity: 0,
          audience: "爱吃辣的 食肉人士",
          price: 30.9,
          ingredient: "猪肉 蒜苗",
          pic: "./hui.jpeg",
        },
        {
          name: "牛肉面",
          detail: "牛肉面牛肉面",
          quantity: 0,
          audience: "面食者 食肉人士",
          price: 15.5,
          ingredient: "面 牛肉",
          pic: "./niu.jpg",
        },
        {
          name: "炒饭",
          detail: "炒饭炒饭",
          quantity: 0,
          price: 18.9,
          ingredient: "大米 鸡蛋",
          pic: "./chao.jpg",
        },
        {
          name: "红烧肉",
          detail:
            "其以五花肉为制作主料，最好选用肥瘦相间的三层肉（五花肉）来做，锅具以砂锅为主，做出来的肉肥瘦相间，香甜松软，营养丰富，入口即化。",
          quantity: 0,
          audience: "食肉人士",
          price: 19.9,
          ingredient: "猪肉",
          pic: "./hong.jpg",
        },
        {
          name: "回锅肉",
          detail:
            "是一种四川传统菜式中家常（味型）菜肴的代表菜肴之一，属于川菜系列。制作原料主要有猪后臀肉、青椒、蒜苗等，口味独特，色泽红亮，肥而不腻。",
          quantity: 0,
          audience: "爱吃辣的 食肉人士",
          price: 30.9,
          ingredient: "猪肉 蒜苗",
          pic: "./hui.jpeg",
        },
        {
          name: "牛肉面",
          detail: "牛肉面牛肉面",
          quantity: 0,
          audience: "面食者 食肉人士",
          price: 15.5,
          ingredient: "面 牛肉",
          pic: "./niu.jpg",
        },
        {
          name: "炒饭",
          detail: "炒饭炒饭",
          quantity: 0,
          price: 18.9,
          ingredient: "大米 鸡蛋",
          pic: "./chao.jpg",
        },
      ],
      foodDetail: {},
      materialDetail:{},
      cartList: [],
    };
  },
  computed: {
    customerCnts() {
      return this.$store.state.cnt;
    },
  },
  methods: {
    showDetail(index) {
      this.showPopup = true;
      this.foodDetail = this.foodList[index];
    },
    showMaterialdetail(index) {
      this.showPopup_2 = true;
      this.materialDetail = this.materialList[index];
    },
    sum() {
      let sum = 0;
      let price = 0;
      for (let i = 0; i < this.foodList.length; i++) {
        sum += this.foodList[i].quantity;
        price += this.foodList[i].quantity * this.foodList[i].price;
      }
      this.foodSum = sum;
      this.priceSum = price.toFixed(1);
      if (sum > this.customerCnts) {
        this.$dialog.alert({
          title: "注意",
          message: "提倡光盘行动，\n 点菜数量超过了用餐人数哦~~",
        });
      }
    },
    showCart() {
      let tmp = [];
      for (let i = 0; i < this.foodList.length; i++) {
        if (this.foodList[i].quantity != 0) {
          tmp.push(this.foodList[i]);
          console.log(this.foodList[i]);
        }
      }
      this.showCartpopup = true;
      this.cartList = tmp;
    },
  },
};
</script>