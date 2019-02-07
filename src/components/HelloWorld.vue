<template>

  <div>
    <h1>计算器</h1>
    <p ref="first">{{first}}</p>
    <p ref="second">{{second}}</p>
    <p ref="operate">{{operate}}</p>
    <p ref="change_result">{{result}}</p>
    <button @click="data1">1</button>
    <button @click="data2">2</button>
    <button @click="jiafa(x,y)">+</button>
    <button @click="result">=</button>

    <!--
    <table>
      <tr>
        <th>data</th>
        <th>...</th>
        <th>...</th>
        <th>operate</th>
      </tr>
      <tr>
        <td>7</td>
        <td>8</td>
        <td>9</td>
        <td id="a">+</td>
      </tr>
      <tr>
        <td>4</td>
        <td>5</td>
        <td>6</td>
        <td id="a">-</td>
      </tr>
      <tr>
        <td>1</td>
        <td>2</td>
        <td>3</td>
        <td id="a">*</td>
      </tr>
      <tr>
        <td>0</td>
        <td>.</td>
        <td id="b">=</td>
        <td id="a">/</td>
      </tr>

    </table>
-->
  </div>

</template>

<script>
export default {
  data() {
    return {
      z: 0,
      first: 0
    }
  },

  methods: {
    data1() {
      console.log('object')
      this.first = 1
    },
    data2() {
      this.second = 2
    },
    jiafa(x, y) {
      document.getElementById('operate').innerHTML = '+'
      this.z = x + y
    },
    result() {
      //  this.result=
    }
  }
}
</script>

<style scoped>
table,
th,
td {
  border: 1px solid blue;
}

table {
  border-collapse: collapse;
}

th {
  background-color: purple;
  color: orange;
  width: 3cm;
  height: 2.5cm;
  font-size: 150%;
}

td {
  height: 2cm;
  text-align: center;
  vertical-align: center;
  font-size: 200%;
  /*padding: 16px;*/
}

#a {
  color: rgb(252, 0, 55);
  background-color: orange;
}

#b {
  color: red;
  background-color: pink;
}
</style>
