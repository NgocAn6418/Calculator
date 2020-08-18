<template>
  <div id="home">
    <div class="calculator">
      <div class="calculator__monitor">
        <span v-model="result" class="calculator__display">{{ result }}</span>
        <input class="calculator__input" readonly type="text" v-model="input" />
      </div>
      <div class="calculator__container">
        <div class="calculator__line">
          <button
            class="calculator__btn"
            v-on:keyup.8="clearCharBtn"
            @click="clearCharBtn"
          >
            CE
          </button>
          <button class="calculator__btn" @click="clearBtn">C</button>
          <button class="calculator__btn" @click="signBtn">+/-</button>
          <button class="calculator__btn" @click="percentBtn">%</button>
          <button class="calculator__btn" @click="divideBtn">/</button>
        </div>
        <div class="calculator__line">
          <button class="calculator__btn" @click="sinBtn">Sin x</button>
          <button class="calculator__btn" @keypress="num7Btn" @click="num7Btn">
            7
          </button>
          <button
            class="calculator__btn"
            @click="num8Btn"
            @keydown.56="num8Btn"
          >
            8
          </button>
          <button
            class="calculator__btn"
            @keydown.57="num9Btn"
            @click="num9Btn"
          >
            9
          </button>
          <button
            class="calculator__btn"
            @keydown.shift.56="multipleBtn"
            @click="multipleBtn"
          >
            *
          </button>
        </div>
        <div class="calculator__line">
          <button class="calculator__btn" @keydown.57="num9Btn" @click="cosBtn">
            Cos x
          </button>
          <button class="calculator__btn" @click="num4Btn">4</button>
          <button class="calculator__btn" @click="num5Btn">5</button>
          <button class="calculator__btn" @click="num6Btn">6</button>
          <button class="calculator__btn" @click="subBtn">-</button>
        </div>
        <div class="calculator__line">
          <button class="calculator__btn" @click="openBtn">(</button>
          <button class="calculator__btn" @click="num1Btn">1</button>
          <button class="calculator__btn" @click="num2Btn">2</button>
          <button class="calculator__btn" @click="num3Btn">3</button>
          <button class="calculator__btn" @click="addBtn">+</button>
        </div>
        <div class="calculator__line">
          <button class="calculator__btn" @click="closeBtn">)</button>
          <button
            class="calculator__btn calculator__btn--zero"
            @click="zeroBtn"
          >
            0
          </button>
          <button class="calculator__btn" @click="floatBtn">.</button>
          <button class="calculator__btn" @keyup.="" @click="equalBtn">
            =
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import { sin, cos, create, all } from "mathjs";

const config = {};
const math = create(all, config);
export default {
  name: "Home",
  data() {
    return {
      input: "0",
      result: "0",
    };
  },
  methods: {
    clearBtn() {
      this.input = "0";
      this.result = "0";
    },
    clearCharBtn() {
      if (this.input !== "0")
        this.input = this.input.slice(0, this.input.length - 1);
    },
    signBtn() {
      var i = this.input.length - 1;
      if (this.input[i] !== ")") {
        var numToSign = this.getLastestNumberAndSlice();
        numToSign = eval(numToSign + "* -1");
        this.input += "(" + numToSign + ")";
      } else {
        var tempNum = "";
        var numToSign = "";
        var count = 1;
        while (i >= 0 && count !== 0) {
          tempNum += this.input[i];
          if (this.input[i] === "(") count--;
          i--;
        }
        for (var k = tempNum.length - 1; k >= 0; k--) numToSign += tempNum[k];
        this.input = this.input.slice(0, i + 1);
        this.input += eval(numToSign + "*-1");
        // for (var k = tempNum.length-k)
        // this.input = this.input.slice(i + 1);
      }
    },
    getLastestNumberAndSlice() {
      var i = this.input.length - 1;
      var numToSignReverse = "";
      var Val = "";
      while ((i !== -1 && /\d/.test(this.input[i])) || this.input[i] === ".") {
        numToSignReverse += this.input[i];
        i--;
      }
      for (let j = numToSignReverse.length - 1; j >= 0; j--)
        Val += numToSignReverse[j];
      this.input = this.input.slice(0, i + 1);
      return Val;
    },
    percentBtn() {
      var numToPercent = this.getLastestNumberAndSlice();
      numToPercent = eval(numToPercent + "/100");
      this.input += numToPercent;
    },
    divideBtn() {
      if (this.input[this.input.length - 1] !== "/") this.input += "/";
    },
    num1Btn() {
      if (this.input !== "0") this.input += "1";
      else this.input = "1";
    },
    num2Btn() {
      if (this.input !== "0") this.input += "2";
      else this.input = "2";
    },
    num3Btn() {
      if (this.input !== "0") this.input += "3";
      else this.input = "3";
    },
    num4Btn() {
      if (this.input !== "0") this.input += "4";
      else this.input = "4";
    },
    num5Btn() {
      if (this.input !== "0") this.input += "5";
      else this.input = "5";
    },
    num6Btn() {
      if (this.input !== "0") this.input += "6";
      else this.input = "6";
    },
    num7Btn() {
      if (this.input !== "0") this.input += "7";
      else this.input = "7";
    },
    num8Btn() {
      if (this.input !== "0") this.input += "8";
      else this.input = "8";
    },
    num9Btn() {
      if (this.input !== "0") this.input += "9";
      else this.input = "9";
    },
    multipleBtn() {
      if (this.input[this.input.length - 1] !== "*") this.input += "*";
    },
    subBtn() {
      if (this.input[this.input.length - 1] !== "-") this.input += "-";
    },
    addBtn() {
      if (this.input[this.input.length - 1] !== "+") this.input += "+";
    },
    floatBtn() {
      var numToFloat = this.getLastestNumberAndSlice();
      if (numToFloat.indexOf(".") === -1) this.input += numToFloat + ".";
      else this.input += numToFloat;
    },
    sinBtn() {
      if (this.input === "0") this.input = "sin(";
      else this.input += "sin(";
    },
    cosBtn() {
      if (this.input === "0") this.input = "cos(";
      else this.input += "cos(";
    },
    openBtn() {
      this.input += "(";
    },
    closeBtn() {
      this.input += ")";

      /*TÍNH LUÔN */
      //   var i = this.input.length - 2;
      //   var count = 1;
      //   var numToCalReverse = "";
      //   var numToCal = "";
      //   while (i >= 0 && count !== 0) {
      //     if (this.input[i] === ")") count++;
      //     if (this.input[i] === "(") count--;
      //     numToCalReverse += this.input[i];
      //     i--;
      //   }
      //   for (var j = numToCalReverse.length - 1; j >= 0; j--) {
      //     numToCal += numToCalReverse[j];
      //   }
      //   if (
      //     count === 0 &&
      //     i >= 0 &&
      //     (this.input[i] === "n" || this.input[i - 1] + this.input[i] === "os")
      //   ) {
      //     if (this.input[i] === "n") numToCal = "sin" + numToCal + "deg)";
      //     if (this.input[i - 1] + this.input[i] === "os")
      //       numToCal = "cos" + numToCal + "deg)";
      //     this.input = this.input.slice(0, i - 2);
      //   }

      //   numToCal = (math.evaluate(numToCal) + "").slice(0, 6);
      //   if (/\d/.test(this.input[this.input.length - 1]))
      //     this.input += "*" + numToCal;
      //   else this.input += numToCal;
      // },
    },
    zeroBtn() {
      var numToAddZero = this.getLastestNumberAndSlice();
      if (
        eval(numToAddZero) !== 0 ||
        numToAddZero.length < 1 ||
        numToAddZero[1] === "."
      )
        this.input += numToAddZero + "0";
      else this.input += numToAddZero;
    },
    equalBtn() {
      try {
        if (this.input !== "") this.result = math.evaluate(this.input);
        else this.result = "0";
        this.input = "";
      } catch (error) {
        this.result = "Error";
      }
    },
  },
};
</script>
<style>
@import url("https://fonts.googleapis.com/css?family=Poppins:300,500&display=swap");
body {
  text-align: center;
  background-color: greenyellow;
}
html {
  height: 100vh;
  width: 100vw;
}
* {
  margin: 0px;
  padding: 0px;
  font-family: "Poppins", sans-serif;
}
.calculator,
.calculator__monitor,
.calculator__container {
  display: flex;
  flex-flow: column nowrap;
  margin: 0px auto;
  border-radius: 10px;
}
.calculator {
  width: 33%;
  background-color: white;
  margin-top: 50px;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
  margin-bottom: 50px;
}
.calculator__monitor,
.calculator__container {
  width: 100%;
}
.calculator__monitor {
  margin-bottom: 20px;
}
.calculator__container {
  background-color: white;
  justify-content: center;
}

.calculator__line {
  display: flex;
  flex-flow: row nowrap;
  justify-content: center;
}
.calculator__btn,
.calculator__btn:active {
  height: 4.5rem;
  width: 4.5rem;
  margin: 5px;
  line-height: 1rem;
  border: none;
  border-radius: 3px;
  background-color: #c5e1a5;
  font-size: 1.2rem;
}
.calculator__input {
  border: none;
  border-bottom: 2px solid green;
  outline: none;
  margin: 10px 21px;
}

.calculator__display {
  text-align: left;
  margin: 0px 20px;
  font-size: 2rem;
}
.calculator__btn:focus {
  outline: none;
}
.calculator__btn:active {
  background-color: #9ccc65;
}

.calculator__btn--zero {
  width: 9.6rem;
  height: 4.5rem;
  padding: 0px 10px;
  margin-bottom: 20px;
}
.calculator__btn--zero:focus {
  width: 9.6rem;
  margin-bottom: 20px;
}
</style>
