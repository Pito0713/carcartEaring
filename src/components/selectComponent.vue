<template>
  <select v-model="selectedIndex">
    <option v-for="(item, index) in source" :value="index" :key="index">
        <!--使用 v-model，要宣告 value props 綁定index-->
      {{ item }}
    </option>
  </select>
</template>

<script>

let selectedIndex = {
  get: function() {
    return this.value;
  },
  set: function(event) {
    this.$emit('input', event)
    // getter 由 value prop 傳進來
    //當改變 <select> 選擇'縣市值'時，setter此時就可使用 this.$emit() 把event 傳出去重新選則的index值。
  }
}

export default {
  name: 'selectComponent',
  props: [
    'value', //v-model 宣告值
    'source' //外部的array資料
  ],
  computed: {
    selectedIndex,
  }
}
</script>