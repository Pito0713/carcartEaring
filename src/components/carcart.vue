<template>
  <div class="orderPage" id="app">
    <div class="cart">
      <div class="cartAllprice">
        <a>總金額 {{allPrice}}元</a>
      </div>
      <div class="cartItem">
        <a v-if="isShow">購物車沒買東西</a>
        <div v-for="(Product,index) in Carts" :key="Product[0]+index" class="cartInfo">
          <div class="cartImg">
            <img :src="Product[7]" />
          </div>
          <div class="cartText">
            <div>
              <a>{{Product[2]}}</a>
              <a>共{{Product[4]}}個</a>
              <p>
                <a>${{Product[3]}}/pcs</a>
                <a>共{{TotalPrice(index)}}</a>
              </p>
            </div>
            <div class="cartButton">
              <div>
                <button @click="addCart (index)">
                  <a>+1</a>
                </button>
                <button @click="cutCart (index)">
                  <a>-1</a>
                </button>
                <i class="far fa-trash-alt" style="padding: 1rem;" @click="trashCan(index)"></i>
              </div>
              <a>庫存剩 {{stk(Product[0])}}</a>
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
                <input
                  type="text"
                  placeholder="姓名"
                  class="menderinput"
                  :class="{ errorborber: NameCheckisError }"
                  v-model="NameCheck"
                />
                <div v-show="NameCheckisError" style="color:red">此欄不能空白</div>
              </div>
            </div>

            <div class="orderInfo">
              <label style="flex:40%">行動電話</label>
              <div style="flex:60%">
                <input
                  type="text"
                  placeholder="電話"
                  class="menderinput"
                  :class="{ errorborber: PhoneCheckisError }"
                  v-model="PhoneCheck"
                />
                <div v-show="PhoneCheckisError" style="color:red">此欄不能空白</div>
              </div>
            </div>

            <div class="orderInfo">
              <label style="flex:40%">E-mail</label>
              <div style="flex:60%">
                <input
                  type="text"
                  placeholder="E-mail"
                  class="menderinput"
                  :class="{ errorborber: MailCheckisError }"
                  v-model="MailCheck"
                />
                <div v-show="MailCheckisError" style="color:red">此欄不能空白</div>
              </div>
            </div>
            <div class="orderInfo">
              <label style="flex:40%">性別</label>
              <div style="flex:60%">
                <input type="radio" name="gender" value="male" style="width:20%; font-size:1rem" />
                <label for="male">男</label>
                <input type="radio" name="gender" value="female" style="width:20%; font-size:1rem" />
                <label for="female">女</label>
              </div>
            </div>
            <div class="orderInfo">
              <label style="flex:40%">地址</label>
              <div style="flex:60%;">
                <twondrop />
                <input type="text" class="menderinput" />
              </div>
            </div>
          </form>
        </div>
      </div>
      <div class="checkButton">
        <button @click="check()">送出訂單</button>
      </div>
    </div>
    <div class="reload" v-show="reload"> <a>重新載入</a></div>
  </div>
</template>

<script>
import $ from "jquery";
import twondrop from "../components/twondrop.vue";
export default {
  data: function() {
    return {
      Carts: [],
      ProdcutData: [],
      isShow: false,
      NameCheck: "",
      NameCheckisError: false,
      PhoneCheck: "",
      PhoneCheckisError: false,
      MailCheck: "",
      MailCheckisError: false,
      reload: false,
    };
  },
  components: {
    twondrop
  },
  watch: {
    NameCheck: function() {
      let vm = this;
      if (vm.NameCheck.length < 1) {
        vm.NameCheckisError = true;
      } else {
        vm.NameCheckisError = false;
      }
    },
    PhoneCheck: function() {
      let vm = this;
      if (vm.PhoneCheck.length < 1) {
        vm.PhoneCheckisError = true;
      } else {
        vm.PhoneCheckisError = false;
      }
    },
    MailCheck: function() {
      let vm = this;
      if (vm.MailCheck.length < 1) {
        vm.MailCheckisError = true;
      } else {
        vm.MailCheckisError = false;
      }
    }
  },
  methods: {
    filterCart() {
      this.Carts = this.Carts.filter(function(item) {
        return item[4] !== ""; // 篩掉其他
      });
      //console.log(this.Carts);
    },
    CartisShow() {
      if (this.Carts.length === 0) {
        this.isShow = !this.isShow;
      }
    },
    TotalPrice(index) {
      let vm = this;
      var Price = vm.Carts[index][3] * vm.Carts[index][4];
      //console.log(Price)
      return Price;
    },
    stk(index) {
      return this.ProdcutData[index][4];
    },
    check() {
      var check = confirm(" 要結帳了嗎");

      if (check) {
        if (this.PhoneCheck === "" || this.NameCheck === "" || this.MailCheck === "") {
          if (this.NameCheck === "") {
            this.NameCheckisError = !this.NameCheckisError;
          }
          if (this.PhoneCheck === "") {
            this.PhoneCheckisError = !this.PhoneCheckisError;
          }
          if (this.MailCheck === "") {
            this.MailCheckisError = !this.MailCheckisError;
          }
        } else {
          for (let i = 0; i < this.Carts.length; i++) {
            this.Carts[i][4] = "";
            //console.log(i);
            //console.log(this.Carts);
            var Cartsdata = [
              [
                this.Carts[i][0],
                this.Carts[i][1],
                this.Carts[i][2],
                this.Carts[i][3],
                this.Carts[i][4],
                this.Carts[i][5],
                this.Carts[i][6],
                this.Carts[i][7],
                this.Carts[i][8],
                this.Carts[i][9]
              ]
            ];
            var Cartsparameter = {};
            Cartsparameter = {
              url:
                "https://docs.google.com/spreadsheets/d/1RJiDnmyc0MZ9ySQy4u8_8PZTJe90LZCkTIs_NCDSjS8/edit#gid=0",
              name: "工作表1",
              data: Cartsdata.toString(),
              row: this.ProdcutData[this.Carts[i][0]][0] + 1,
              column: this.ProdcutData[this.Carts[i][0]].length
            };
            $.get(
              "https://script.google.com/macros/s/AKfycbz-k7jYi1VMPguXmuvf7W2cZFb39JZD9_QnnuBYbH9Okm5vb4Ui/exec",
              Cartsparameter
            );
          }
          alert("訂單送出了!");
          this.timer = setTimeout(() => {
            //延遲讓後台更新
            window.location.reload();
          }, 1000);
        }
      }
    },
    addCart(index) {
      let vm = this;
      if (vm.ProdcutData[vm.Carts[index][0]][4] - 1 >= 0) {
        vm.$set(vm.Carts[index], [4], vm.Carts[index][4] + 1);
        //console.log(vm.Carts[index][4]);
        vm.$set(
          vm.ProdcutData[vm.Carts[index][0]],
          [4],
          vm.ProdcutData[vm.Carts[index][0]][4] - 1
        );
        //console.log(vm.ProdcutData[vm.Carts[index][0]]);
      } else {
        alert("老闆庫存不夠惹");
      }

      vm.TotalPrice(index);

      var Cartsdata = [
        [
          vm.Carts[index][0],
          vm.Carts[index][1],
          vm.Carts[index][2],
          vm.Carts[index][3],
          vm.Carts[index][4],
          vm.Carts[index][5],
          vm.Carts[index][6],
          vm.Carts[index][7],
          vm.Carts[index][8],
          vm.Carts[index][9]
        ]
      ];
      var Cartsparameter = {};
      Cartsparameter = {
        url:
          "https://docs.google.com/spreadsheets/d/1RJiDnmyc0MZ9ySQy4u8_8PZTJe90LZCkTIs_NCDSjS8/edit#gid=0",
        name: "工作表1",
        data: Cartsdata.toString(),
        row: vm.ProdcutData[vm.Carts[index][0]][0] + 1,
        column: vm.ProdcutData[vm.Carts[index][0]].length
      };
      $.get(
        "https://script.google.com/macros/s/AKfycbz-k7jYi1VMPguXmuvf7W2cZFb39JZD9_QnnuBYbH9Okm5vb4Ui/exec",
        Cartsparameter
      );

      var Prodcutdata = [
        [
          vm.ProdcutData[vm.Carts[index][0]][0],
          vm.ProdcutData[vm.Carts[index][0]][1],
          vm.ProdcutData[vm.Carts[index][0]][2],
          vm.ProdcutData[vm.Carts[index][0]][3],
          vm.ProdcutData[vm.Carts[index][0]][4],
          vm.ProdcutData[vm.Carts[index][0]][5],
          vm.ProdcutData[vm.Carts[index][0]][6],
          vm.ProdcutData[vm.Carts[index][0]][7],
          vm.ProdcutData[vm.Carts[index][0]][8],
          vm.ProdcutData[vm.Carts[index][0]][9]
        ]
      ];
      var Prodcutparameter = {};
      Prodcutparameter = {
        url:
          "https://docs.google.com/spreadsheets/d/1nXquMbDuBjMx2Eo7qO1XBKNrJBm8xNGRGexuOFozlts/edit#gid=0",
        name: "工作表1",
        data: Prodcutdata.toString(),
        row: vm.ProdcutData[vm.Carts[index][0]][0] + 1, // execl第2行開始
        column: vm.ProdcutData[vm.Carts[index][0]].length
      };
      $.get(
        "https://script.google.com/macros/s/AKfycbxQ5_HzD8ow_wRBH839AXXptKL_JqbA1DsiO55iwsL33pyhshUA/exec",
        Prodcutparameter
      );
    },
    cutCart(index) {
      let vm = this;
      if (vm.Carts[index][4] - 1 > 0) {
        vm.$set(vm.Carts[index], [4], vm.Carts[index][4] - 1);
        //console.log(vm.Carts[index][4]);
        vm.$set(
          vm.ProdcutData[vm.Carts[index][0]],
          [4],
          vm.ProdcutData[vm.Carts[index][0]][4] + 1
        );
        //console.log(vm.ProdcutData[vm.Carts[index][0]]);
      } else {
        var yes = confirm("你確定不要了嗎");
        if (yes) {
          alert("移出購物車了");

          vm.$set(
            vm.ProdcutData[vm.Carts[index][0]],
            [4],
            vm.ProdcutData[vm.Carts[index][0]][4] + 1
          );
          vm.$set(vm.Carts[index], [4], "");
          //console.log(vm.ProdcutData[vm.Carts[index][0]]);
          this.reload = !this.reload
          this.timer = setTimeout(() => {
            //延遲讓後台更新
            window.location.reload();
          }, 1000);
        }
      }

      vm.TotalPrice(index);

      var Cartsdata = [
        [
          vm.Carts[index][0],
          vm.Carts[index][1],
          vm.Carts[index][2],
          vm.Carts[index][3],
          vm.Carts[index][4],
          vm.Carts[index][5],
          vm.Carts[index][6],
          vm.Carts[index][7],
          vm.Carts[index][8],
          vm.Carts[index][9]
        ]
      ];
      var Cartsparameter = {};
      Cartsparameter = {
        url:
          "https://docs.google.com/spreadsheets/d/1RJiDnmyc0MZ9ySQy4u8_8PZTJe90LZCkTIs_NCDSjS8/edit#gid=0",
        name: "工作表1",
        data: Cartsdata.toString(),
        row: vm.ProdcutData[vm.Carts[index][0]][0] + 1,
        column: vm.ProdcutData[vm.Carts[index][0]].length
      };
      $.get(
        "https://script.google.com/macros/s/AKfycbz-k7jYi1VMPguXmuvf7W2cZFb39JZD9_QnnuBYbH9Okm5vb4Ui/exec",
        Cartsparameter
      );

      var Prodcutdata = [
        [
          vm.ProdcutData[vm.Carts[index][0]][0],
          vm.ProdcutData[vm.Carts[index][0]][1],
          vm.ProdcutData[vm.Carts[index][0]][2],
          vm.ProdcutData[vm.Carts[index][0]][3],
          vm.ProdcutData[vm.Carts[index][0]][4],
          vm.ProdcutData[vm.Carts[index][0]][5],
          vm.ProdcutData[vm.Carts[index][0]][6],
          vm.ProdcutData[vm.Carts[index][0]][7],
          vm.ProdcutData[vm.Carts[index][0]][8],
          vm.ProdcutData[vm.Carts[index][0]][9]
        ]
      ];
      var Prodcutparameter = {};
      Prodcutparameter = {
        url:
          "https://docs.google.com/spreadsheets/d/1nXquMbDuBjMx2Eo7qO1XBKNrJBm8xNGRGexuOFozlts/edit#gid=0",
        name: "工作表1",
        data: Prodcutdata.toString(),
        row: vm.ProdcutData[vm.Carts[index][0]][0] + 1, // execl第2行開始
        column: vm.ProdcutData[vm.Carts[index][0]].length
      };
      $.get(
        "https://script.google.com/macros/s/AKfycbxQ5_HzD8ow_wRBH839AXXptKL_JqbA1DsiO55iwsL33pyhshUA/exec",
        Prodcutparameter
      );
    },
    trashCan(index) {
      let vm = this;
      var yes = confirm("你確定不要了嗎");
        if (yes) {
          alert("移出購物車了");

          vm.$set(
            vm.ProdcutData[vm.Carts[index][0]],
            [4],
            vm.ProdcutData[vm.Carts[index][0]][4] + vm.Carts[index][4]
          );
          vm.$set(vm.Carts[index], [4], "");
          //console.log(vm.ProdcutData[vm.Carts[index][0]]);
          this.reload = !this.reload
          this.timer = setTimeout(() => {
            //延遲讓後台更新
            window.location.reload();
          }, 1000);
        }
    
      vm.TotalPrice(index);

      var Cartsdata = [
        [
          vm.Carts[index][0],
          vm.Carts[index][1],
          vm.Carts[index][2],
          vm.Carts[index][3],
          vm.Carts[index][4],
          vm.Carts[index][5],
          vm.Carts[index][6],
          vm.Carts[index][7],
          vm.Carts[index][8],
          vm.Carts[index][9]
        ]
      ];
      var Cartsparameter = {};
      Cartsparameter = {
        url:
          "https://docs.google.com/spreadsheets/d/1RJiDnmyc0MZ9ySQy4u8_8PZTJe90LZCkTIs_NCDSjS8/edit#gid=0",
        name: "工作表1",
        data: Cartsdata.toString(),
        row: vm.ProdcutData[vm.Carts[index][0]][0] + 1,
        column: vm.ProdcutData[vm.Carts[index][0]].length
      };
      $.get(
        "https://script.google.com/macros/s/AKfycbz-k7jYi1VMPguXmuvf7W2cZFb39JZD9_QnnuBYbH9Okm5vb4Ui/exec",
        Cartsparameter
      );

      var Prodcutdata = [
        [
          vm.ProdcutData[vm.Carts[index][0]][0],
          vm.ProdcutData[vm.Carts[index][0]][1],
          vm.ProdcutData[vm.Carts[index][0]][2],
          vm.ProdcutData[vm.Carts[index][0]][3],
          vm.ProdcutData[vm.Carts[index][0]][4],
          vm.ProdcutData[vm.Carts[index][0]][5],
          vm.ProdcutData[vm.Carts[index][0]][6],
          vm.ProdcutData[vm.Carts[index][0]][7],
          vm.ProdcutData[vm.Carts[index][0]][8],
          vm.ProdcutData[vm.Carts[index][0]][9]
        ]
      ];
      var Prodcutparameter = {};
      Prodcutparameter = {
        url:
          "https://docs.google.com/spreadsheets/d/1nXquMbDuBjMx2Eo7qO1XBKNrJBm8xNGRGexuOFozlts/edit#gid=0",
        name: "工作表1",
        data: Prodcutdata.toString(),
        row: vm.ProdcutData[vm.Carts[index][0]][0] + 1, // execl第2行開始
        column: vm.ProdcutData[vm.Carts[index][0]].length
      };
      $.get(
        "https://script.google.com/macros/s/AKfycbxQ5_HzD8ow_wRBH839AXXptKL_JqbA1DsiO55iwsL33pyhshUA/exec",
        Prodcutparameter
      );
    }
  },
  computed: {
    allPrice: function() {
      var all = 0;
      for (let i = 0; i < this.Carts.length; i++) {
        all += this.Carts[i][3] * this.Carts[i][4];

        //console.log(a)
      }
      return all;
    }
  },
  mounted() {
    fetch(
      "https://script.google.com/macros/s/AKfycbwzGM7BJ8SnGD626ebzQi3xGdBsJzUlOSdiDIkMmBhplN65FtQ/exec"
    )
      .then(res => {
        return res.json();
      })
      .then(Products => {
        this.ProdcutData = Products;
        //console.log(this.ProdcutData);
        fetch(
          "https://script.google.com/macros/s/AKfycbxLQARlHh9k7LbV8-ORSmVjIYAJtgphhKXFS0e6ypXmpAWJX8cV/exec"
        )
          .then(res => {
            return res.json();
          })
          .then(Carts => {
            this.Carts = Carts;
            this.filterCart();
            this.CartisShow();
            //console.log(this.Carts);
          });
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
.cartItem {
  overflow: auto;
  height: 500px;
}
.cartAllprice {
  padding: 1rem;
  font-size: 1.2rem;
  border-bottom: 1px solid var(--border-color);
}
.cartInfo {
  display: flex;
  margin: 1rem;
}
.orderInfo {
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
  .menderinput {
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
  justify-content: space-between;
  align-items: flex-end;
}
.checkButton {
  display: flex;
  justify-content: flex-end;

  button {
    padding: 0.5rem;
    font-size: 1.2rem;
  }
}
.errorborber {
  border: 2px solid red;
}
.reload{
  display: block;
  position: fixed;
  top:50%;
  left: calc(50% - 4rem);
  background-color: var(--background-color);
  font-size: 2rem;
}
@media screen and (min-width: 1024px) {
  .cartAllprice {
    padding: 1vw;
    font-size: 2.2vw;
  }
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
