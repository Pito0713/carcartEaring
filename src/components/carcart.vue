<template>
  <div class="orderPage" id="app">
    <div class="cart">
      <div v-for="(Product,index) in Carts" :key="Product[0]+index" class="cartInfo">
        <div class="cartImg">
          <img :src="Product[7]" />
        </div>
        <div class="cartText">
          <div>
            <a>{{Product[2]}}</a>
            <a>共{{Product[4]}}個</a>
            <a>{{TotalPrice(index)}}</a>
          </div>
          <div class="cartButton">
            <button>
              <a>+1</a>
            </button>
            <button>
              <a>-1</a>
            </button>
            <button>
              <a>送出訂單</a>
            </button>
          </div>
        </div>
      </div>
    </div>
    <div class="mender">
      <div class="mendertable">
        <div>
          <a>訂購人資料</a>
        </div>
        <div>
          <form action>
            <div class="orderInfo">
              <label style="flex:40%">姓名</label>
              <div style="flex:60%">
                  <input  type="text" placeholder="姓名" class="menderinput" />
              </div>
              
            </div>

            <div class="orderInfo">
              <label style="flex:40%">行動電話</label>
              <div style="flex:60%">
                  <input  type="text" placeholder="電話" class="menderinput" 
              :class="{ errorborber: isError }"
              v-model="phoneCheck"
              />
              <div v-show="isError" style="color:red">電話號碼有誤</div>
              </div>
              
            </div>

            <div class="orderInfo">
              <label style="flex:40%">E-mail</label>
              <div style="flex:60%"> 
                  <input  type="text" placeholder="E-mail" class="menderinput" />
              </div>
              
            </div>
            <div class="orderInfo">
              <label style="flex:40%">性別</label>
              <div style="flex:60%">
                <input type="radio" value="男" style="width:20%; font-size:1rem" />男
                <input type="radio" value="女" style="width:20%; font-size:1rem" />女
              </div>
            </div>
            <div class="orderInfo">
              <label style="flex:40%">地址</label>
              <div style="flex:60%">
                <select id="country" name="country">
                  <option value="australia">Aus</option>
                  <option value="canada">Canada</option>
                </select>
                縣
                <select id="country" name="country">
                  <option value="australia">Aus</option>
                </select>
                村
                <select id="country" name="country">
                  <option value="australia">Aus</option>
                </select>
                鄉
                <select id="country" name="country">
                  <option value="australia">Aus</option>
                </select>
                街
              </div>
            </div>
          </form>
        </div>
      </div>
      <div>
          <button>修改訂單</button>
      </div>
    </div>
  </div>
</template>

<script>
//import $ from 'jquery'
export default {
  data: function() {
    return {
      Carts: [],
      phoneCheck: '',
      isError:false,
    };
  },
  watch:{
      phoneCheck:function(){
          let vm =this
          if(vm.phoneCheck.length < 10){
            vm.isError = true
          } else {
            vm.isError = false;
          }
      }
          
    },
  methods: {
    filterCart() {
      this.Carts = this.Carts.filter(function(item) {
        return item[4] !== ""; // 篩掉其他
      });
      console.log(this.Carts);
    },
    TotalPrice(index) {
        //let vm = this;
        console.log(index)
        //console.log(this.Carts[i][3])
        //console.log(this.Carts[i][4])
        var Price = this.Carts[index][3] * this.Carts[index][4];
        console.log(Price)
        return Price;
    }
  },
  computed: {
  },
  mounted() {
    fetch(
      "https://script.google.com/macros/s/AKfycbxLQARlHh9k7LbV8-ORSmVjIYAJtgphhKXFS0e6ypXmpAWJX8cV/exec"
    )
      .then(res => {
        return res.json();
      })
      .then(Carts => {
        this.Carts = Carts;
        this.filterCart();
        //console.log(this.Carts);
      });
  }
};
</script>

<style  scoped lang="scss">
button {
  margin: 1vw;
  background-color: #ffffff;
  border: var(--border-color) 1px solid;
  color: var(--plat-color);
}
.orderPage {
  display: flex;
  height: auto;
}
.cart {
  display: flex;
  flex: 50%;
  flex-direction: column;
  overflow: auto;
  height: 500px;
}
.cartInfo {
  display: flex;
  height: 100px;
  margin: 1rem;
}
.orderInfo{
    display: flex;
    padding: 0.5rem;
}
.mender {
  display: flex;
  flex: 50%;
  padding: 2vw;
  flex-direction: column;
}
.mendertable {
  border: 1px solid var(--border-color);
  padding: 2vw;
  form {
    width: 100%;
  }
  .menderinput{
      width: 100%;
  }
}
.cartImg {
  flex: 40%;
  img {
    max-width: 100%;
    max-height: 100%;
  }
}
.cartText {
  flex: 60%;
  display: flex;
  height: 100%;
  flex-direction: column;
  justify-content: space-between;
}
.cartButton {
  display: flex;
  justify-content: flex-end;
}
.errorborber{
    border:2px solid red;
}
@media screen and (max-width: 765px) {
  .orderPage {
    flex-direction: column-reverse;
    height: 100%;
  }
  .cart {
    flex: 100%;
  }
  .mender {
    flex: 100%;
  }
}
</style>
