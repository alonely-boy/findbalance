<template>
  <div class="container">
    <div>请输入数组(支持以逗号，空格分格字符):</div>
    <div><input type="text" v-model.trim="inputText" /></div>
    <div>您的输入数组为:</div>
    <div class="arr">{{ getArr }}</div>
    <button @click="calculateBalance">计算平衡点</button>
    <div>找到的平衡点下标为(-1表示没找到):</div>
    <div>{{ ans }}</div>
  </div>
</template>
<script>
export default {
  name: "App",
  components: {},
  data() {
    return {
      inputText: "", // -7 1 5 2 -4 3 0 测试数据
      ans: -1,
    };
  },
  methods: {
    calculateBalance() {
      this.ans = -1; //初始化结果
      let arr = this.getArr;
      if (arr) {
        try {
          //throw Error终止forEach
          arr.forEach((item) => {
            if (typeof item !== "number" || Number.isNaN(item)) {
              throw Error("输入数组有误，请检查!");
            }
          });
        } catch (error) {
          alert(error);
          return;
        }

        /*创建左数组，右数组，左数组元素为从左至右元素和，右数组同理，为计算数组和，需
        初始化第一个元素为0
      */
        let leftArr = [],
          rightArr = [],
          len = arr.length,
          ans = [];

        leftArr[0] = 0;
        rightArr[len - 1] = 0;
        for (let i = 1; i < len; i++) {
          leftArr[i] = leftArr[i - 1] + arr[i - 1];
        }
        for (let i = len - 2; i >= 0; i--) {
          rightArr[i] = rightArr[i + 1] + arr[i + 1];
        }
        for (let i = 0; i < len; i++) {
          if (leftArr[i] == rightArr[i]) {
            ans.push(i);
          }
        }
        if (ans.length) {
          this.ans = ans;
        } else {
          this.ans = -1;
        }
      }
    },
  },
  computed: {
    getInput() {
      let reg = /(,+)|(\s+)|(，+)/g;
      return this.inputText.replace(reg, " ").replace(/\s+/g, " ");
    },
    getArr() {
      let input = this.getInput;
      if (input.length) {
        let arr = input.split(" ").map((item) => {
          if (item === "") {
            return null;
          }
          return Number(item);
        });
        return arr;
      }
    },
  },
};
</script>

<style lang="scss">
.container {
  border: 1px solid black;
  padding: 0 10px;
  width: 500px;
  min-height: 350px;
  position: absolute;
  left: 50%;
  margin-top: 20px;
  transform: translate(-50%, 0);
  div {
    background: white;
    border-radius: 2px;
    margin-top: 15px;
    margin-bottom: 15px;
    padding-right: 10px;
    input {
      width: 100%;
      line-height: 100%;
    }
  }
}
</style>
