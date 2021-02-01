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
      <table class="mendertable" width="100%">
        <tr>
          <td>
            <a>訂購人資料</a>
          </td>
        </tr>
        <tr>
          <td>
            <form action>
              <tr>
                <td width=30%>
                  <a>姓名</a>
                </td>
                <td width=100%>   
                  <input type="text" placeholder="姓名" class="menderinput"/>
                </td>
              </tr>
              <tr>
                <td>
                  <label>電話</label>
                </td>
                <td>  
                  <input type="text" placeholder="電話" class="menderinput"/>
                </td>
              </tr>
              <tr>
                <td>
                  <label>E-mail</label>
                  </td>
                <td> 
                  <input type="text" placeholder="E-mail" class="menderinput"/>
                </td>
              </tr>
              <tr>
                <td>
                  <label>性別</label>
                  </td>
                <td> 
                  <input type="radio" value="男" style="width:20%; font-size:1rem"/>男
                  <input type="radio" value="女" style="width:20%; font-size:1rem"/>女
                </td>
              </tr>
              <tr>
                <td>
                  <label>地址</label>
                </td>
              </tr>
              <tr>  
                <td>
                    <select id="country" name="country">
                        <option value="australia">Aus</option>
                        <option value="canada">Canada</option>
                        <option value="usa">USA</option>
                    </select> 
                    縣  
                    <select id="country" name="country">
                        <option value="australia">Aus</option>
                        <option value="canada">Canada</option>
                        <option value="usa">USA</option>
                    </select> 
                    鄉
                </td>
              </tr>
            </form>
          </td>
        </tr>
      </table>
    </div>
  </div>
</template>

<script>
//import $ from 'jquery'
export default {
  data: function() {
    return {
      Products: [],
      ProductData: {},
      Carts: []
    };
  },
  methods: {
    filterCart() {
      this.Carts = this.Carts.filter(function(item) {
        return item[4] !== ""; // 篩掉其他
      });
      console.log(this.Carts);
    }
  },
  computed: {},
  mounted() {
    fetch(
      "https://script.google.com/macros/s/AKfycbxguddnzylMny9C4bu2lm3ojmd_NYQPw2HjfzmbrPVnV9laIX4/exec"
    )
      .then(res => {
        return res.json();
      })
      .then(Products => {
        this.Products = Products;
        this.ProductDataStorage = Products[0][4];
      });
    fetch(
      "https://script.google.com/macros/s/AKfycbxLQARlHh9k7LbV8-ORSmVjIYAJtgphhKXFS0e6ypXmpAWJX8cV/exec"
    )
      .then(res => {
        return res.json();
      })
      .then(Carts => {
        this.Carts = Carts;
        this.filterCart();
        console.log(this.Carts);
      });
  }
};
</script>

<style  scoped lang="scss">
.orderPage {
  display: flex;
  height: 40vw;
}
.cart {
  display: flex;
  flex: 50%;
  flex-direction: column;
  overflow: auto;
}
.cartInfo {
  display: flex;
  height: 100px;
  margin: 1rem;
}
.mender {
  display: flex;
  flex: 50%;
}
.mendertable {
  border: 1px solid var(--border-color);
  form {
      width: 100%;
  }
  td label{
    font-size: 1.2rem;  
  }
  td input{
      height: 1.5rem
  }
  .menderinput{
      width: 90%;
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
