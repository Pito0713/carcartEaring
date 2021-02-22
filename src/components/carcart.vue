<template>
  <div class="orderPage" id="app">
    <div class="cart">
      <div class="cartAllprice"><a>總金額  {{allPrice}}元</a></div>
      <div class="cartItem">
      <div v-for="(Product,index) in Carts" :key="Product[0]+index" class="cartInfo">
        <div class="cartImg">
          <img :src="Product[7]" />
        </div>
        <div class="cartText">
          <div>
            <a>{{Product[2]}}</a>
            <a>共{{Product[4]}}個</a>
            <p>每個${{Product[3]}}  <a style="margin: 0 2rem;">  共{{TotalPrice(index)}}</a></p>
            
          </div>
          <div class="cartButton">
            <button @click="addCart (index)">
              <a>+1</a>
            </button>
            <button @click="cutCart (index)">
              <a>-1</a>
            </button>
            <button @click="coverCartBackData(index)">
              <a>修改訂單</a>
            </button>
          </div>
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
                  <input  type="text" placeholder="姓名" class="menderinput" 
                  :class="{ errorborber: NameCheckisError }"
              v-model="NameCheck"
              />
              <div v-show="NameCheckisError" style="color:red">此欄不能空白</div>
              
              </div>
              
            </div>

            <div class="orderInfo">
              <label style="flex:40%">行動電話</label>
              <div style="flex:60%">
                  <input  type="text" placeholder="電話" class="menderinput" 
              :class="{ errorborber: PhoneCheckisError }"
              v-model="PhoneCheck"
              />
              <div v-show="PhoneCheckisError" style="color:red">此欄不能空白</div>
              </div>
              
            </div>

            <div class="orderInfo">
              <label style="flex:40%">E-mail</label>
              <div style="flex:60%"> 
                  <input  type="text" placeholder="E-mail" class="menderinput" 
                  :class="{ errorborber: MailCheckisError }"
                  v-model="MailCheck"
                  />
                  <div v-show="MailCheckisError" style="color:red">此欄不能空白</div>
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
              <div style="flex:60%;">
                <twondrop />
                <input type="text" class="menderinput">
              </div>
            </div>
          </form>
        </div>
      </div>
      <div>
          <button>確認訂單</button>
      </div>
    </div>
  </div>
</template>

<script>
import $ from 'jquery'
import twondrop from '../components/twondrop.vue'
export default {
  data: function() {
    return {
      Carts: [],
      NameCheck: '',
      NameCheckisError:false,
      PhoneCheck: '',
      PhoneCheckisError:false,
      MailCheck: '',
      MailCheckisError:false,
    };
  },
  components:{
    twondrop 
  },
  watch:{
      PhoneCheck:function(){
          let vm =this
          if(vm.PhoneCheck.length < 1){
            vm.PhoneCheckisError = true
          } else {
            vm.PhoneCheckisError = false;
          }
      },
      MailCheck:function(){
          let vm =this
          if(vm.MailCheck.length < 1){
            vm.MailCheckisError = true
          } else {
            vm.MailCheckisError = false;
          }
      },
      
          
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
        //console.log(index)
        //console.log(this.Carts[i][3])
        //console.log(this.Carts[i][4])
        var Price = this.Carts[index][3] * this.Carts[index][4];
        //console.log(Price)
        return Price;
    },
    addCart (index) {
      //if (this.Carts[index][4] === 0) {
        //alert('庫存不夠惹')
      //} else {
        //this.ProductDataStorage = this.Carts[index][4] + 1 // 暫存
        //this.Carts[index][4] = this.ProductDataStorage // 取代實際量
        
        this.$set(this.Carts[index],[4],this.Carts[index][4] + 1)
        console.log(this.Carts[index][4])
        this.TotalPrice(index)
      //}
    },
    cutCart (index) {
      //if (this.ProductDataStorageStk > 0) {
        //this.ProductDataStorage = this.Carts[index][4] - 1 // 暫存
        //this.Carts[index][4] = this.ProductDataStorage // 取代實際量
        this.$set(this.Carts[index],[4],this.Carts[index][4] - 1)
        console.log(this.Carts[index][4])

        this.TotalPrice(index)
      //}
    },
    coverCartBackData (index) {
      console.log(this.Carts)
      var data = [[
        this.Carts[index][0],
        this.Carts[index][1],
        this.Carts[index][2],
        this.Carts[index][3],
        this.Carts[index][4],
        this.Carts[index][5],
        this.Carts[index][6],
        this.Carts[index][7],
        this.Carts[index][8],
        this.Carts[index][9]
      ]]
      //var parameter = {}
      //parameter = {
        //url: 'https://docs.google.com/spreadsheets/d/1nXquMbDuBjMx2Eo7qO1XBKNrJBm8xNGRGexuOFozlts/edit#gid=0',
        //name: '工作表1',
        //data: data.toString(),
        //row: this.Carts[index][0] + 1, // execl第2行開始
        //column: this.Carts[index].length
      //}
      //$.get('https://script.google.com/macros/s/AKfycbxQ5_HzD8ow_wRBH839AXXptKL_JqbA1DsiO55iwsL33pyhshUA/exec', parameter)
      // 覆蓋暫存的EaringbackData
      var parameter = {}
      parameter = {
        url: 'https://docs.google.com/spreadsheets/d/1RJiDnmyc0MZ9ySQy4u8_8PZTJe90LZCkTIs_NCDSjS8/edit#gid=0',
        name: '工作表1',
        data: data.toString(),
        row: this.Carts[index][0] + 1,
        column: this.Carts[index].length
      }
      
      $.get('https://script.google.com/macros/s/AKfycbz-k7jYi1VMPguXmuvf7W2cZFb39JZD9_QnnuBYbH9Okm5vb4Ui/exec', parameter)
    

      //this.timer = setTimeout(()=>{  //延遲讓後台更新
        //window.location.reload()
      //},800);
    },




     



  },
  computed: {
    //TotalPrice (index) {
      //var Price = this.Carts[index][3] * this.Carts[index][4];
        //console.log(Price)
      //return Price;
    //}
    allPrice:function (){
      var all = 0
      for(let i = 0;i < this.Carts.length;i++){
        all +=this.Carts[i][3] * this.Carts[i][4];
        
        //console.log(a)
      }
      return all
    }
    
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
  
}
.cartItem{
  overflow: auto;
  height: 600px;
}
.cartAllprice{
  padding: 1rem;
  font-size: 1.2rem;
  border-bottom: 1px solid var(--border-color)
}
.cartInfo {
  display: flex;
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
      width: 50%;
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
  padding: 1rem;
  flex: 60%;
  display: flex;
  height: 100%;
  flex-direction: column;
  justify-content: space-between;
}
.cartButton {
  display: flex;
  justify-content: flex-end;
  align-items: flex-end;
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
