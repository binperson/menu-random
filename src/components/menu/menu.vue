<template>
  <div class="menu">
    <div class="menu-part menu-list">
      <div class="menu-title">
        <h1>{{menuTitle}}</h1>
        <input @click="submit" class="search-text" @keydown.enter="submit" type="number" v-model.number="num" :placeholder="filterName">
      </div>
      <ul>
        <li v-for="(item,index) in allFoods" class="food-item">
          <div class="icon">
            <img :src="imgs[index%3]" width="209" height="156" alt="">
          </div>
          <div class="content">
            <div class="name">{{item.name}}</div>
            <div class="old-price">¥{{item.price}}</div>
            <div class="desc">Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incidicunt ut labore et tumagna aliqua</div>
            <div class="price">Delivery cost: <span class="price-detail">¥{{Math.round(Math.random()*9+1)}}</span></div>
            <div @click="addFood(item)" class="cartcontrol-add">Add to Order</div>
          </div>
        </li>
      </ul>
    </div>
    <div class="menu-part menu-order">
      <div class="order-title">
        <i class="icon-order"></i>
        <span>
        Your Order
        </span>
      </div>
      <div v-if="filterName !== 'Total prices'">
        <ul v-if="foods.length > 0">
          <li v-for="(item, index) in foods" class="order-item">{{index+1}} 、 {{item.name}} ¥{{item.price}} <i @click="decreaseFood(index)" class="icon-reduce"></i></li>
        </ul>
        <div class="none-food" v-if="foods.length === 0">...</div>
        <div class="total">Total<span class="total-detail">¥{{totalPrice}}</span></div>
      </div>
      <div v-if="filterName === 'Total prices'">
        <div class="item-wrapper" v-for="items in foods">
          <ul>
            <li v-for="(item, index) in items" class="order-item">{{index+1}} 、 {{item.name}} ¥{{item.price}} <i @click="decreaseFood(index)" class="icon-reduce"></i></li>
          </ul>
          <div class="total">Total<span class="total-detail">¥{{totalItemsPrice(items)}}</span></div>
        </div>
        <div v-if="foods.length == 0">
          <div class="none-food" v-if="foods.length === 0">...</div>
          <div class="total">Total<span class="total-detail">¥{{totalPrice}}</span></div>
        </div>
      </div>

      <button>Checkout</button>
    </div>

  </div>
</template>

<script type="text/ecmascript-6">
  import axios from 'axios'
  export default {
      props: {
        filterName: {
            type: String
        },
        menuTitle: {
            type: String
        }
      },
      data(){
        return {
          imgs: ['http://47.93.25.215:8080/static/img/food-icon-one.86e5bde.jpg','http://47.93.25.215:8080/static/img/food-icon-two.9ae9ac5.jpg','http://47.93.25.215:8080/static/img/food-icon-three.e7c96fa.jpg'],
          allFoods: [],
          foods: [],
          num: ''
        }
      },
      created() {
        let that = this;
        let url = this.HOST + '/menu/getAll'
        axios.get(url)
          .then(function (response) {
            console.log(response);
            that.allFoods = response.data.data;

          })
          .catch(function (response) {
            console.log(response);
          });
      },
      computed: {
        totalPrice() {
          let sum = 0;
          for(let i = 0; i < this.foods.length; i++ ){
            sum += this.foods[i].price
          }
          return sum;
        }
      },
      methods: {
        submit() {
          if(this.num === ''){
            return;
          }
          let that = this;
          if(this.filterName === 'Num of dishes'){
            let url = this.HOST + '/menu/index'
            axios.get(url+'?num='+this.num)
              .then(function (response) {
                console.log(response);
                that.foods = response.data.data;
              })
              .catch(function (response) {
                console.log(response);
              });
          }
          if(this.filterName === 'Total prices'){
            let url = this.HOST + '/menu/specifyPrice'
            axios.get(url+'?price='+this.num)
              .then(function (response) {

                console.log(response);
                that.foods = response.data.data;
                console.log(response.data.data);
                console.log(that.foods);
              })
              .catch(function (response) {
                console.log(response);
              });
          }

        },
        addFood(item){

            this.foods.push(item);
        },
        decreaseFood(index){
          console.log(11);
          this.foods.splice(index, 1);
        },
        totalItemsPrice(items){
          let sum =0;
          for(let i = 0; i < items.length; i++){
            sum += items[i].price;
          }
          return sum;
        }
      }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .menu
    padding: 8px 26px
    display: flex
    position: absolute
    top: 0
    left: 250px
    right: 0
    bottom: 0
    .menu-part
      &.menu-list
        flex: 1
      &.menu-order
        flex: 0 0 234px
    .menu-title
      position: relative
      margin: 0 18px 0 0
      border-bottom: 1px solid #dddddd
      h1
        height: 60px
        color: #d7634e
        line-height: 60px
        font-size: 20px
      .search-text
        background-image: url(./icon-search.png)
        background-size: 14px 14px
        background-color: #fff;
        background-repeat: no-repeat
        background-position: 7px 50%
      input
        position: absolute
        right: 0
        bottom: 15px
        outline: 0
        border-radius: 4px
        border: 1px solid #dddddd
        width: 165px
        padding: 5px 5px 5px 25px
        color: #d7d7d7
    .food-item
      margin: 20px 18px 20px 0
      display: flex
      padding-bottom: 20px
      border-bottom: 1px solid #dddddd
      .icon
        flex: 0 0 209px
        margin-right: 24px
      .content
        flex: 1
        position: relative
        .name
          margin: 10px 0 0px 0
          height: 34px
          line-height: 34px
          font-size: 16px
          color: #d7634e
        .old-price
          color: #95c255
          position: absolute
          top: 17px
          right: 0
        .desc
          color: #b3b3b3
          line-height: 20px
          font-size: 16px
          margin-bottom: 10px
        .price
          color: #d7634e
          font-size: 16px
          .price-detail
            color: #adadad
            font-size: 16px
        .cartcontrol-add
          font-size: 16px
          padding: 10px 32px
          background: #d7634e
          display: inline-block
          color: #ffffff
          border-radius: 5px
          position: absolute;
          right: 0
          bottom: 8px
    .menu-order
      margin: 0px 16px 0 18px
      .item-wrapper
        border-bottom: 1px solid #dddddd
      .order-title
        height: 60px
        line-height: 60px
        color: #d7634e
        font-size: 16px
        border-bottom: 1px solid #dddddd
      .order-item
        position: relative
        font-size: 14px
        line-height: 54px
        height: 54px
        color: #adadad
        border-bottom: 1px solid #dddddd
        &:last-child
          border-bottom:none
        .icon-reduce
          position: absolute
          right: 0
          bottom: 18px
          font-size: 18px
      .none-food
        font-size: 14px
        color: #adadad
        text-align: center
        line-height: 54px
        height: 54px
      .total
        position: relative
        font-size: 16px
        color: #adadad
        margin-bottom: 20px
        .total-detail
          color: #95c255
          font-size: 16px
          position: absolute
          right: 0
          bottom: 0
      button
        display: block
        cursor: pointer
        background: #95c255
        color: #ffffff
        border: none
        width: 100%
        border-radius: 5px
        padding: 12px 0
        font-size: 18px
</style>
