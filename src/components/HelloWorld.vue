<template>

  <div>
    <h1>计算器</h1>

    <!-- <button v-for="(item,idx) in ui"
            :key="idx"
            @click="onClick(item  )">
      {{item}}
    </button>
-->
    <!--Table-->
    <table align="center">
      <tr>
        <td class="equation_s"
            colspan="4"
            id="text_equation">equation</td>
      </tr>
      <tr>
        <td class="equation_s"
            colspan="4"
            id="text_result">result</td>
      </tr>
      <tr>
        <td @click="onClick('CE')">CE</td>
        <td @click="onClick('C')">C</td>
        <td @click="onClick('BS')">BS</td>
        <td @click="onClick('➗')">➗</td>
      </tr>
      <tr>
        <td @click="onClick('7')">7</td>
        <td @click="onClick('8')">8</td>
        <td @click="onClick('9')">9</td>
        <td @click="onClick('✖')">✖</td>
      </tr>
      <tr>
        <td @click="onClick('4')">4</td>
        <td @click="onClick('5')">5</td>
        <td @click="onClick('6')">6</td>
        <td @click="onClick('➖')">➖</td>
      </tr>
      <tr>
        <td @click="onClick('1')">1</td>
        <td @click="onClick('2')">2</td>
        <td @click="onClick('3')">3</td>
        <td @click="onClick('➕')">➕</td>
      </tr>
      <tr>
        <td @click="onClick('-')">±</td>
        <td @click="onClick('0')">0</td>
        <td @click="onClick('.')">.</td>
        <td @click="onClick('=')">=</td>
      </tr>

    </table>
  </div>

</template>

<script>
export default {
  data() {
    return {
      //  ui: [1, 2, 3, 4, 5, 6, 7, 8, 9, 0, "+", "-", "*", "/", "="],
      equation: "", //算式
      result: 0, //结果
      text: "", //字符
      x: 0,
      xx: 0, //x的缓存
      x_n: 0, //x的余数
      x_bit: 0, //x的位数
      x_float: 0, //x是小数时，的整数部分
      x_float_bit: 0, //x是小数时，整数部分的位数
      x_float_n: 0, //判断x是不是小数
      x_float_x: 1, //x的小数部分操作用到的数
      x_float_x_s: 1, //因为x的小数部分的的位数，包含小数点的那一位，所以从1开始
      symbol: "",
      symbol_n: 0, //符号的缓存
      symbol_sign: "",
      bs_n1: 0,
      bs_n2: 0
    };
  },

  methods: {
    add(a, b) {
      return a + b;
    }, //加
    subtract(a, b) {
      return a - b;
    }, //减
    multiply(a, b) {
      return a * b;
    }, //乘
    divide(a, b) {
      return a / b;
    }, //除
    float() {
      //如果是小数时，先把第一次输入的数和位数，保存起来，
      //再把小数点之后的数除以他自身的10倍，得到小数部分
      //最后把，两部分加起来
      this.x_float_x_s = 1;
      this.x_float_x = 1;
      for (; this.x_float_x_s < this.x_bit; ) {
        this.x_float_x *= 10;
        this.x_float_x_s++;
      }
      this.x /= this.x_float_x;
      this.x += this.x_float;
      this.x_bit += this.x_float_bit;
      this.x_float = 0;
      this.x_float_bit = 0;
    },
    number() {
      //如果输入的是数字，把它自身扩大10倍，加上下一个输入的数
      if (
        this.text == "9" ||
        this.text == "8" ||
        this.text == "7" ||
        this.text == "6" ||
        this.text == "5" ||
        this.text == "4" ||
        this.text == "3" ||
        this.text == "2" ||
        this.text == "1" ||
        this.text == "0" ||
        this.text == "." ||
        this.text == "-" //若输入的是负号时，把负号保存起来，在进行运算的时候，再把负号添上计算
      ) {
        switch (this.text) {
          case "0":
            this.x = this.x * 10 + 0;
            break;
          case "1":
            this.x = this.x * 10 + 1;
            break;
          case "2":
            this.x = this.x * 10 + 2;
            break;
          case "3":
            this.x = this.x * 10 + 3;
            break;
          case "4":
            this.x = this.x * 10 + 4;
            break;
          case "5":
            this.x = this.x * 10 + 5;
            break;
          case "6":
            this.x = this.x * 10 + 6;
            break;
          case "7":
            this.x = this.x * 10 + 7;
            break;
          case "8":
            this.x = this.x * 10 + 8;
            break;
          case "9":
            this.x = this.x * 10 + 9;
            break;
          case ".":
            this.x_float = this.x;
            this.x = 0;
            this.x_float_bit = this.x_bit;
            this.x_bit = 0;
            this.x_float_n++;
            break;
          case "-":
            this.symbol_sign = this.text; //若输入的是负号时，把负号保存起来，在进行运算的时候，再把负号添上计算
            break;
          default:
            break;
        }
        this.text = "";
        this.x_bit++;
      }
    },
    data_operate() {
      //若输入的是加减乘除，根据加减号的个数，分别进行不同的操作
      if (
        this.text == "➕" ||
        this.text == "➖" ||
        this.text == "✖" ||
        this.text == "➗"
      ) {
        if (this.x_float_n == 1) {
          //在进行运算之前，判断是否有小数参与运算，若是，复原小数
          this.float();
          this.x_float_n = 0;
        }
        if (this.symbol_sign == "-") {
          //判断是否有负号，若是，添上去
          this.x = 0 - this.x;
          this.symbol_sign = "";
          this.x_bit--; //减去输入负号时，数字位数增加的一位
        }
        if (this.symbol_n < 1) {
          //第一次出现加减..号，直接把数字存起来，碰到下一个加减..号，或者=号时运算
          this.xx = this.x;
        }
        if (this.symbol_n == 1) {
          //第一个和第二个数进行计算
          switch (this.symbol) {
            case "➕":
              this.result = this.add(this.xx, this.x);
              break;
            case "➖":
              this.result = this.subtract(this.xx, this.x);
              break;
            case "✖":
              this.result = this.multiply(this.xx, this.x);
              break;
            case "➗":
              this.result = this.divide(this.xx, this.x);
              break;
            default:
              break;
          }
        }
        if (this.symbol_n > 1) {
          //第三个以后，直接在上一步算出的结果上计算
          switch (this.symbol) {
            case "➕":
              this.result = this.add(this.result, this.x);
              break;
            case "➖":
              this.result = this.subtract(this.result, this.x);
              break;
            case "✖":
              this.result = this.multiply(this.result, this.x);
              break;
            case "➗":
              this.result = this.divide(this.result, this.x);
              break;
            default:
              break;
          }
        }
        this.symbol_n++;
        this.symbol = this.text; //把上一次碰到的加减..号存起来，并且每次都是
        this.x = 0;
        this.text = "";
        this.x_bit = 0;
      }
      document.getElementById("text_result").innerHTML = this.result;
    },
    operate() {
      //CE，C，Backspace，=
      if (
        this.text == "BS" ||
        this.text == "CE" ||
        this.text == "C" ||
        this.text == "="
      ) {
        if (this.x_float_n == 1) {
          //如果是小数，并且直接等号时
          this.float();
          this.x_float_n = 0;
        }
        switch (this.text) {
          case "BS": //Backspace 退格,好麻烦
            if (this.equation.length > 0) {
              this.equation = this.equation.substring(
                0,
                this.equation.length - 3 //这里因为算式加上了BS两个字母，再有一个退格，就是3
              );
            }
            document.getElementById("text_equation").innerHTML = this.equation;
            if (this.bs_n2 < 1) {
              if (this.x != 0) {
                this.x_n = this.x % 10;
                this.x -= this.x_n;
                this.x /= 10;
              } //如果是数字的话，相当于是反着来
              //这里有两种情况，输入数字，然后退格，bs_n1==1,就不会执行符号的退格操作
              //如果是加减号，bs_n2==1,就不会执行数字的退格操作
              this.bs_n1++;
              this.bs_n2 = 0;
              this.x_bit--; //数字的位数减1
            }
            if (this.bs_n1 < 1) {
              if (
                this.symbol == "➕" ||
                this.symbol == "➖" ||
                this.symbol == "✖" ||
                this.symbol == "➗"
              ) {
                this.symbol = "";
                this.symbol_n--;
              } //如果只是算式上的退格是不行的，你输入的符号已经缓存了，这里需要清除缓存，和修改一些设置。
              this.bs_n2++;
              this.bs_n1 = 0;
            }
            break;
          case "CE": //清除输入的数字（包括负号）
            if (this.symbol_sign == "-") {
              this.symbol_sign = "";
            }
            if (this.equation.length > 0) {
              this.equation = this.equation.substring(
                0,
                this.equation.length - this.x_bit - 2 //这里因为算式加上了BS两个字母，再有数字的位数
              );
            }
            document.getElementById("text_equation").innerHTML = this.equation;
            this.x = 0;
            this.x_bit = 0;
            break;
          case "C": //清除计算器
            this.equation = ""; //算式
            this.result = 0; //结果
            this.text = ""; //字符
            this.x = 0;
            this.xx = 0; //x的缓存
            this.x_n = 0; //x的余数
            this.x_bit = 0; //x的位数
            this.x_float = 0; //x是小数时，的整数部分
            this.x_float_bit = 0; //x是小数时，整数部分的位数
            this.x_float_n = 0; //判断x是不是小数
            this.x_float_x = 1; //x的小数部分操作用到的数
            this.x_float_x_s = 1; //因为x的小数部分的的位数，包含小数点的那一位，所以从1开始
            this.symbol = "";
            this.symbol_n = 0; //符号的缓存
            this.symbol_sign = "";
            this.bs_n1 = 0;
            this.bs_n2 = 0;
            document.getElementById("text_equation").innerHTML = this.equation;
            break;
          case "=": //计算结果
            if (this.symbol_sign == "-") {
              this.x = 0 - this.x;
              this.symbol_sign = "";
              this.x_bit--;
            }
            if (this.symbol_n < 1) {
              this.result = this.x;
            }
            if (this.symbol_n == 1) {
              switch (this.symbol) {
                case "➕":
                  this.result = this.add(this.xx, this.x);
                  break;
                case "➖":
                  this.result = this.subtract(this.xx, this.x);
                  break;
                case "✖":
                  this.result = this.multiply(this.xx, this.x);
                  break;
                case "➗":
                  this.result = this.divide(this.xx, this.x);
                  break;
                default:
                  break;
              }
            }
            if (this.symbol_n > 1) {
              switch (this.symbol) {
                case "➕":
                  this.result = this.add(this.result, this.x);
                  break;
                case "➖":
                  this.result = this.subtract(this.result, this.x);
                  break;
                case "✖":
                  this.result = this.multiply(this.result, this.x);
                  break;
                case "➗":
                  this.result = this.divide(this.result, this.x);
                  break;
                default:
                  break;
              }
            }
            this.x = 0;
            this.xx = 0;
            this.x_bit = 0;
            this.symbol = "";
            this.symbol_n = 2;
            this.equation = this.result;
            break;
          default:
            break;
        }
        this.text = "";
        document.getElementById("text_result").innerHTML = this.result;
      }
    },
    //有一点问题就是，不能正确的计算出两个小数的运算，比如1.3+2.3=3.5999999999999996
    //其他功能基本正常
    onClick(key) {
      this.text = key;
      this.equation = this.equation + this.text; //把每次按键的结果连在算式后
      document.getElementById("text_equation").innerHTML = this.equation; //显示算式
      this.number(); //改变数字
      this.data_operate(); //数字操作
      this.operate(); //操作
    }
  }
};
</script>

<style scoped>
table,
td {
  border: 1px solid blue;
  border-collapse: collapse;
}

td {
  width: 100px;
  height: 70px;
  text-align: center;
  vertical-align: center;
  background-color: gray;
  color: black;
  font-size: 200%;
  /*padding: 16px;*/
}

.equation_s {
  background-color: gainsboro;
}
</style>
