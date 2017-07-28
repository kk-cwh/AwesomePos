<template>
  <div class="pos">
    <div>
      <el-row>
        <el-col :span='7' class='pos-order' id='order-list'>
          <el-tabs>
            <el-tab-pane label="点餐">
              <el-table :data="tableData" show-summary border :summary-method="getSummaries" style="width: 100%">
  
                <el-table-column prop="goodsName" label="商品"></el-table-column>
                <el-table-column prop="count" label="量" width="60"></el-table-column>
                <el-table-column prop="price" label="金额" width="90"></el-table-column>
                <el-table-column label="操作" width="100" fixed="right">
                  <template scope='scope'>
                    <el-button type="text" size="small" @click='deleteRow(scope.$index, tableData)'>删除</el-button>
                    <el-button type="text" size="small" @click='add_goods_num(scope.$index,tableData)'>增加</el-button>
                  </template>
  
                </el-table-column>
              </el-table>
              <div class="div-btn">
                <el-button type="warning">挂单</el-button>
                <el-button type="danger" @click='delAllGoods'>删除</el-button>
                <el-button type="success" @click='checkout'>结账</el-button>
              </div>
  
            </el-tab-pane>
            <el-tab-pane label="挂单">
              挂单
            </el-tab-pane>
            <el-tab-pane label="外卖">
              外卖
            </el-tab-pane>
          </el-tabs>
        </el-col>
        <!--商品展示-->
        <el-col :span="17">
          <div>
            <div class="title">常用商品</div>
            <div class="often-goods-list">
              <ul>
                <li v-for="goods in oftenGoods" @click='add_to_order(goods)'>
                  <span v-text="goods.goodsName"></span>
                  <span class="o-price" v-text="'￥'+ goods.price+'元'"></span>
                </li>
              </ul>
            </div>
          </div>
          <div class="goods-type">
  
            <el-tabs>
              <el-tab-pane label="汉堡">
                <div>
                  <ul class="cookList">
                    <li v-for="goods in type0Goods" @click='add_to_order(goods)'>
                      <span class="foodImg">
                        <img :src="goods.goodsImg" width="100%" />
                      </span>
                      <span>{{goods.goodsName}}</span>
                      <span>￥{{goods.price}}元</span>
                    </li>
  
                  </ul>
                </div>
              </el-tab-pane>
              <el-tab-pane label="小食">
                <div>
                  <ul class="cookList">
                    <li v-for="goods in type1Goods" @click='add_to_order(goods)'>
                      <span class="foodImg">
                        <img :src="goods.goodsImg" width="100%" />
                      </span>
                      <span>{{goods.goodsName}}</span>
                      <span>￥{{goods.price}}元</span>
                    </li>
  
                  </ul>
                </div>
              </el-tab-pane>
              <el-tab-pane label="饮料">
                <div>
                  <ul class="cookList">
                    <li v-for="goods in type2Goods" @click='add_to_order(goods)'>
                      <span class="foodImg">
                        <img :src="goods.goodsImg" width="100%" />
                      </span>
                      <span>{{goods.goodsName}}</span>
                      <span>￥{{goods.price}}元</span>
                    </li>
  
                  </ul>
                </div>
              </el-tab-pane>
              <el-tab-pane label="套餐">
                <div>
                  <ul class="cookList">
                    <li v-for="goods in type3Goods" @click='add_to_order(goods)'>
                      <span class="foodImg">
                        <img :src="goods.goodsImg" width="100%" />
                      </span>
                      <span>{{goods.goodsName}}</span>
                      <span>￥{{goods.price}}元</span>
                    </li>
  
                  </ul>
                </div>
              </el-tab-pane>
  
            </el-tabs>
          </div>
  
        </el-col>
      </el-row>
    </div>
  </div>
  </div>
</template>

<script>
import axios from 'Axios'
export default {
  name: 'pos',
  data: function () {
    return {
      tableData: [],
      oftenGoods: [],
      type0Goods: [
        {
          "goodsId": 1,
          "goodsImg": "http://7xjyw1.com1.z0.glb.clouddn.com/pos001.jpg",
          "goodsName": "香辣鸡腿堡",
          "price": 18
        },
        {
          "goodsId": 2,
          "goodsImg": "http://7xjyw1.com1.z0.glb.clouddn.com/pos002.jpg",
          "goodsName": "田园鸡腿堡",
          "price": 15
        },
        {
          "goodsId": 3,
          "goodsImg": "http://7xjyw1.com1.z0.glb.clouddn.com/pos003.jpg",
          "goodsName": "和风汉堡",
          "price": 15
        }
      ],
      type1Goods: [
        {
          "goodsId": 4,
          "goodsImg": "http://7xjyw1.com1.z0.glb.clouddn.com/pos003.jpg",
          "goodsName": "大包薯条",
          "price": 18
        },
        {
          "goodsId": 5,
          "goodsImg": "http://7xjyw1.com1.z0.glb.clouddn.com/pos002.jpg",
          "goodsName": "脆皮炸鸡腿",
          "price": 20
        },
        {
          "goodsId": 6,
          "goodsImg": "http://7xjyw1.com1.z0.glb.clouddn.com/pos001.jpg",
          "goodsName": "魔法鸡块",
          "price": 20
        }
      ],
      type2Goods: [
        {
          "goodsId": 7,
          "goodsImg": "http://7xjyw1.com1.z0.glb.clouddn.com/pos001.jpg",
          "goodsName": "可乐大杯",
          "price": 10
        },
        {
          "goodsId": 8,
          "goodsImg": "http://7xjyw1.com1.z0.glb.clouddn.com/pos002.jpg",
          "goodsName": "雪顶咖啡",
          "price": 18
        }
      ],
      type3Goods: [
        {
          "goodsId": 9,
          "goodsImg": "http://7xjyw1.com1.z0.glb.clouddn.com/pos004.jpg",
          "goodsName": "儿童欢乐套餐",
          "price": 25
        },
        {
          "goodsId": 10,
          "goodsImg": "http://7xjyw1.com1.z0.glb.clouddn.com/pos003.jpg",
          "goodsName": "快乐全家桶",
          "price": 99
        }
      ],
    }
  },
  created() {
    // axios.get('http://jspang.com/DemoApi/oftenGoods.php')
    //   .then(response => {
    //     this.oftenGoods = response.data;
    //   })
    this.oftenGoods = [
      {
        goodsId: 1,
        goodsName: "香辣鸡腿堡",
        price: 18
      },
      {
        goodsId: 2,
        goodsName: "田园鸡腿堡",
        price: 15
      },
      {
        goodsId: 3,
        goodsName: "和风汉堡",
        price: 15
      },
      {
        goodsId: 4,
        goodsName: "大包薯条",
        price: 18
      },
      {
        goodsId: 5,
        goodsName: "脆皮炸鸡腿",
        price: 20
      },
      {
        goodsId: 6,
        goodsName: "魔法鸡块",
        price: 20
      },
      {
        goodsId: 7,
        goodsName: "可乐大杯",
        price: 10
      },
      {
        goodsId: 8,
        goodsName: "雪顶咖啡",
        price: 18
      },
      {
        goodsId: 9,
        goodsName: "儿童欢乐套餐",
        price: 25
      },
      {
        goodsId: 10,
        goodsName: "快乐全家桶",
        price: 99
      }
    ];
    // axios.get('http://jspang.com/DemoApi/typeGoods.php')
    //   .then(response => {
    //     this.type0Goods = response.data[0];
    //     this.type1Goods = response.data[1];
    //     this.type2Goods = response.data[2];
    //     this.type3Goods = response.data[3];

    //   })
    //   .catch(error => {
    //     console.log(error);
    //     alert('网络错误，不能访问');
    //   })
  }
  ,
  mounted() {
    var orderHeight = document.body.clientHeight;
    document.getElementById("order-list").style.height = orderHeight + 'px';


  },
  methods: {
    delAllGoods() {
      this.tableData = [];
    },
    deleteRow(index, rows) {
      rows.splice(index, 1);
    },
    add_goods_num(index, rows) {
      rows[index].count++;
      rows.splice(index, 1, rows[index]);
    },
    getSummaries(param) {
      const sums = [];
      sums[0] = '总价';
      sums[1] = 0;
      sums[2] = 0;
      var orders = this.tableData;
      var flag = false;
      for (let i = 0; i < orders.length; i++) {
        sums[1] += orders[i].count;
        sums[2] += (orders[i].count * orders[i].price);
      }
      sums[2] = '￥' + sums[2];
      return sums;
    },
    add_to_order(goods) {

      var orders = this.tableData;
      var flag = false;
      for (let i = 0; i < orders.length; i++) {
        if (orders[i].goodsId == goods.goodsId) {
          orders[i].count++;
          flag = true;
          this.tableData.splice(i, 1, orders[i]);
          break;
        }
      }
      if (!flag) {
        goods.count = 1;
        this.tableData.push(goods);
      }
    },
    checkout() {
      if (this.tableData.length != 0) {
        this.tableData = [];
        this.$message({
          message: '结账成功，感谢你又为店里出了一份力!',
          type: 'success'
        });

      } else {
        this.$message.error('不能空结。老板了解你急切的心情！');
      }

    }

  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.pos-order {
  background-color: #F9FAFC;
  border-right: 1px solid #C0CCDA;
}

.div-btn {
  margin-top: 10px;
  text-align: center
}

.title {
  height: 20px;
  border-bottom: 1px solid #D3DCE6;
  background-color: #F9FAFC;
  padding: 10px;
}

.often-goods-list ul li {
  list-style: none;
  float: left;
  border: 1px solid #E5E9F2;
  padding: 10px;
  margin: 5px;
  background-color: #fff;
}

.o-price {
  color: #58B7FF;
}

.goods-type {
  clear: both;
}

.cookList li {
  list-style: none;
  width: 23%;
  border: 1px solid #E5E9F2;
  height: auot;
  overflow: hidden;
  background-color: #fff;
  padding: 2px;
  float: left;
  margin: 2px;
}

.cookList li span {
  display: block;
  float: left;
}

.foodImg {
  width: 40%;
}

.foodName {
  font-size: 18px;
  padding-left: 10px;
  color: brown;
}

.foodPrice {
  font-size: 16px;
  padding-left: 10px;
  padding-top: 10px;
}
</style>
