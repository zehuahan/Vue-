<script setup>
import { onBeforeMount, reactive } from "vue";
// 排序列表
const menuList = reactive([
  { name: "销量从高到低", statu: 1, type: "sales" },
  { name: "价格从低到高", statu: 0, type: "price" },
  { name: "价格从高到低", statu: 0, type: "price" },
]);
// 商品列表
const shopList = reactive([
  {
    price: 6000,
    title: "宣传册设计/招商画册设计宣传册设计/招商画册设计",
    score: "4.0",
    sales: 87621,
  },
  {
    price: 5000,
    title: "宣传册设计/招商画册设计",
    score: "4.4",
    sales: 75895,
  },
  {
    price: 7000,
    title: "宣传册设计/招商画册设计宣传册设计/招商画册设计",
    score: "4.6",
    sales: 91565,
  },
  {
    price: 8000,
    title: "宣传册设计/招商画册设计",
    score: "3.5",
    sales: 76562,
  },
]);
// 头部分类
const List = reactive([
  {
    title: "服务分类",
    list: [
      { name: "全部", statu: 1 },
      { name: "金融服务", statu: 0 },
      { name: "人才与培训服务", statu: 0 },
      { name: "知识产权服务", statu: 0 },
      { name: "商务服务", statu: 0 },
      { name: "互联网和相关服务", statu: 0 },
      { name: "软件和信息技术服务", statu: 0 },
      { name: "法律服务", statu: 0 },
      { name: "技术创新服务", statu: 0 },
      { name: "空间服务", statu: 0 },
      { name: "小试中试", statu: 0 },
      { name: "市场营销", statu: 0 },
      { name: "检验检测服务", statu: 0 },
      { name: "其他服务", statu: 0 },
    ],
  },
  {
    title: "优惠活动",
    list: [
      { name: "全部", statu: 1 },
      { name: "服务券产品", statu: 0 },
      { name: "特惠产品", statu: 0 },
      { name: "现实免费产品", statu: 0 },
    ],
  },
]);
// 已选分类
let selectList = reactive([]);
function addSelect() {
  for (let i in List) {
    for (let k in List[i].list) {
      if (List[i].list[k].statu == 1) {
        selectList.push(List[i].list[k]);
      }
    }
  }
}
// 商品列表排序
function sortArr(arr, attr = "sales", flag = true) {
  for (var i = 0; i < arr.length - 1; i++) {
    //比较arr.length-1轮
    for (var j = i + 1; j < arr.length; j++) {
      if (arr[i][attr] < arr[j][attr] && flag == true) {
        //交换
        var temp = arr[i]; //临时变量
        arr[i] = arr[j];
        arr[j] = temp;
      } else if (arr[i][attr] > arr[j][attr] && flag == false) {
        var temp = arr[i]; //临时变量
        arr[i] = arr[j];
        arr[j] = temp;
      }
    }
  }
  console.log(arr);
  return arr;
}
onBeforeMount(() => {
  addSelect();
  sortArr(shopList);
});
// 选择分类
function hasClicked(item) {
  if (item.statu == 0) {
    item.statu = 1;
    selectList.push(item);
  } else {
    item.statu = 0;
    selectList.splice(selectList.find(item), 1);
  }
}
// 重置按钮
function reset() {
  selectList.splice(0, selectList.length);
  for (let i in List) {
    for (let k in List[i].list) {
      if (List[i].list[k].statu == 1) {
        List[i].list[k].statu = 0;
      }
    }
  }
}
// 删除按钮
function del(item, index) {
  selectList.splice(index, 1);
  for (let i in List) {
    if (List[i].list[List[i].list.indexOf(item)]) {
      List[i].list[List[i].list.indexOf(item)].statu = 0;
    }
  }
}
// 排序列表
function chengeClick(item, index) {
  for (let i in menuList) {
    if (menuList[i].statu == 1) {
      menuList[i].statu = 0;
    }
  }
  menuList[index].statu = 1;
  if (item.name == "价格从低到高") {
    sortArr(shopList, item.type, false);
  } else {
    sortArr(shopList, item.type, true);
  }
}
</script>

<template>
  <div class="root">
    <div class="top">
      <div v-for="(item, index) in List" :key="index">
        <p>{{ item.title }}</p>
        <ul>
          <li
            v-for="(item, index) in item.list"
            :key="index"
            :class="item.statu == 1 ? 'active' : ''"
            @click="hasClicked(item)"
          >
            {{ item.name }}
          </li>
        </ul>
      </div>
      <form>
        <input type="text" placeholder="请输入服务名称" />
        <button>搜索</button>
      </form>
    </div>
    <div class="middle">
      <p>已选条件</p>
      <ul>
        <li v-for="(item, index) in selectList" :key="index">
          {{ item.name }}&nbsp;&nbsp;&nbsp;<span @click="del(item, index)"
            >x</span
          >
        </li>
      </ul>
      <button @click="reset()">重置</button>
    </div>
    <div class="content">
      <div class="title">
        <p class="titleLeft">已为您找到20款服务商品</p>
        <ul class="titleRight">
          <li
            :class="item.statu == 1 ? 'active' : ''"
            v-for="(item, index) in menuList"
            :key="index"
            @click="chengeClick(item, index)"
          >
            {{ item.name }}
          </li>
        </ul>
      </div>
      <div class="contentList">
        <div class="contentItem" v-for="(item, index) in shopList" :key="index">
          <img
            src="https://lanhu.oss-cn-beijing.aliyuncs.com/SketchPng8e30bda308381f9818caa359e6ac40e00ccf707323c5998583a9b182ccb84138"
            alt=""
          />
          <div class="text">
            <p class="price">￥{{ item.price }}</p>
            <p class="title">{{ item.title }}</p>
            <div>
              <span class="score">评分：{{ item.score }}</span
              ><span class="sales">{{ item.sales }}次交易</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped lang="scss">
.root {
  max-width: 1200px;
  width: 1200px;
  margin: 0 auto;
  position: relative;
}
.top {
  border: 1px solid #ededed;
  border-radius: 3px;
  background: #fff;
  width: 100%;
  margin-top: 37px;
  height: 164px;
  p {
    color: #9d9d9d;
    width: 110px;
    float: left;
    text-align: center;
  }
  ul {
    display: flex;
    flex-flow: row wrap;
    list-style: none;
    li {
      margin: 2px 10px;
      padding: 8px 10px;
      :first {
        margin: 0;
      }
    }
    :hover {
      cursor: pointer;
      color: #d42a26;
    }
    .active {
      color: #fff;
      background: #d32521;
      border-radius: 5px;
    }
  }
  form {
    width: 250px;
    position: relative;
    top: -56px;
    left: 950px;
    input {
      padding: 10px;
      position: relative;
      z-index: 2;
      border: 1px solid #ddd;
      border-radius: 5px 0 0 5px;
    }
    button {
      position: absolute;
      padding: 10px;
      border-radius: 0 5px 5px 0;
      margin-left: -2px;
      color: #fff;
      background: #d32521;
      border: none;
    }
  }
}
.middle {
  border: 1px solid #ededed;
  border-radius: 3px;
  background: #fff;
  width: 100%;
  margin-top: 12px;
  height: 64px;
  position: relative;
  p {
    width: 110px;
    float: left;
    text-align: center;
    line-height: 64px;
    margin: 0;
  }
  ul {
    display: flex;
    flex-flow: row wrap;
    list-style: none;
    margin-top: 11px;
    max-width: 900px;
    height: 44px;
    overflow-x: scroll;
    overflow-y: hidden;
    li {
      line-height: 24px;
      margin: 0 10px;
      padding: 10px;
      color: #d32925;
      background: #fdf5f4;
      border-radius: 5px;
    }
    :hover {
      cursor: pointer;
    }
  }
  ::-webkit-scrollbar {
    display: none;
  }
  button {
    padding: 10px;
    border: 1px solid #ededed;
    border-radius: 5px;
    background: #fff;
    position: absolute;
    right: 15px;
    top: 14px;
  }
}
.content {
  width: 100%;
  display: flex;
  flex-flow: column nowrap;
  .title {
    width: 100%;
    height: 54px;
    display: flex;
    justify-content: space-between;
    .titleLeft {
      font-size: 20px;
    }
    .titleRight {
      display: flex;
      justify-content: space-between;
      width: 300px;
      list-style: none;
      font-size: 16px;
      li:hover {
        cursor: pointer;
      }
      .active {
        color: #d32521;
      }
    }
  }
  .contentList {
    width: 100%;
    display: flex;
    justify-content: space-between;
    .contentItem {
      width: 20%;
      box-shadow: 0px 5px 10px rgba(0, 0, 0, 0.3);
      img {
        width: 100%;
      }
      .text {
        padding: 10px;
        .price {
          font-size: 18px;
          font-weight: bold;
          color: #d32521;
        }
        div {
          display: flex;
          justify-content: space-between;
          font-size: 14px;
          .score {
            color: #dd5451;
          }
          .sales {
            color: #a3a3a3;
          }
        }
      }
    }
  }
}
</style>
