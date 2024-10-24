<template>
  <div id="app">
    <div class="container">
      <table>
        <tr>
          <td colspan="5">
            <div id="screen">
              <span id="screen_top" v-text="lastMemory">M=0</span>
              <div id="screen_bottom">
                <!-- v-text is a directive that is used to replace the content of HTML tag with private data -->
                <!-- It will update the content automatically when data is changed. It is called data reactive -->
                <span v-text="lastInput" id="operand1">0</span>
                <span id="operator"></span>
                <span id="operand2"></span>
              </div>
              <!-- <span id="screen_bottom">0</span> -->
            </div>
          </td>
        </tr>
        <tr>
          <td>
            <!-- <button type="button" class="btn btn-warning">MC</button> -->

            <!-- On a calculator, "MC" clears the memory, "MR" recalls the number stored in memory, 
             "M-" subtracts the displayed number from the memory, and "M+" adds the displayed number to the memory;
              essentially allowing you to store and manipulate numbers during a calculation sequence -->
            <MyButton
              @click="clearMemory"
              color="btn-warning"
              :label="'MC'"></MyButton>
          </td>
          <td>
            <!-- <button type="button" class="btn btn-warning">MR</button> -->

            <MyButton
              @click="recallMemory"
              color="btn-warning"
              :label="'MR'"></MyButton>
          </td>
          <td>
            <!-- <button type="button" class="btn btn-warning">M-</button> -->
            <MyButton
              @click="substractMemory"
              color="btn-warning"
              :label="'M-'"></MyButton>
          </td>
          <td>
            <!-- <button type="button" class="btn btn-warning">M+</button> -->

            <MyButton
              @click="addMemory"
              color="btn-warning"
              :label="'M+'"></MyButton>
          </td>
          <td>
            <button type="button" @click="deleteLast" class="btn btn-light">
              <i class="fa fa-long-arrow-right" aria-hidden="true"></i>
            </button>
          </td>
        </tr>
        <tr>
          <td>
            <MyButton @click="showNumber(7)" :label="7"></MyButton>
          </td>
          <td>
            <MyButton @click="showNumber(8)" :label="8"></MyButton>
          </td>
          <td>
            <MyButton @click="showNumber(9)" :label="9"></MyButton>
          </td>
          <td>
            <MyButton
              @click="setOperate('/')"
              color="btn-secondary"
              :label="'÷'"></MyButton>
          </td>
          <td>
            <!-- <button type="button" class="btn btn-light">+/-</button> -->
            <MyButton @click="toggleSign" :label="'+/-'"></MyButton>
          </td>
        </tr>
        <tr>
          <td>
            <MyButton @click="showNumber(4)" :label="4"></MyButton>
          </td>
          <td>
            <MyButton @click="showNumber(5)" :label="5"></MyButton>
          </td>
          <td>
            <MyButton @click="showNumber(6)" :label="6"></MyButton>
          </td>
          <td>
            <!-- <button type="button" class="btn btn-secondary">x</button> -->
            <MyButton
              @click="setOperate('*')"
              color="btn-secondary"
              :label="'x'"></MyButton>
          </td>
          <td>
            <!-- <button type="button" class="btn btn-secondary">-</button> -->
            <MyButton
              color="btn-secondary"
              @click="setOperate('-')"
              :label="'-'"></MyButton>
          </td>
        </tr>
        <tr>
          <td>
            <!-- <button
              v-on:click="showNumber(1)"
              type="button"
              class="btn btn-light"
            >
              1
            </button> -->

            <MyButton @click="showNumber(1)" :label="1"></MyButton>
          </td>
          <td>
            <MyButton @click="showNumber(2)" :label="2"></MyButton>
          </td>
          <td>
            <MyButton @click="showNumber(3)" :label="3"></MyButton>
          </td>
          <td rowspan="2">
            <!-- <button type="button" class="btn btn-secondary long-btn">+</button> -->
            <MyButton
              color="btn-secondary"
              class="long-btn"
              @click="setOperate('+')"
              :label="'+'"></MyButton>
          </td>
          <td rowspan="2">
            <!-- <button type="button" class="btn btn-primary long-btn">=</button> -->

            <MyButton
              color="btn-primary"
              class="long-btn"
              @click="calculate"
              :label="'='"></MyButton>
          </td>
        </tr>
        <tr>
          <td>
            <MyButton @click="clear" color="btn-danger" :label="'C'"></MyButton>
          </td>
          <td>
            <MyButton @click="showNumber(0)" :label="0"></MyButton>
          </td>
          <td>
            <MyButton @click="inputDecimal" :label="'.'"></MyButton>
          </td>
        </tr>
      </table>
    </div>
    <div class="alert alert-danger" id="message_panel" role="alert">
      something wrong here
    </div>
  </div>
</template>

<script>
import MyButton from "./components/MyButton.vue";
export default {
  name: "App",
  components: {
    MyButton,
  },
  data() {
    return {
      // This is the private data section which can be used inside this component
      lastInput: "",
      temp: "",
      operator: null,
      memory: 0,
      lastMemory :  "M = 0"
     
    };
  },
  methods: {
    showNumber(number) {
      // Assign number when user click to the lastInput data
      // To access private data from methods, use (this.)
      // append the number
      this.lastInput = this.lastInput.toString() + number.toString();
      console.log(this.lastInput);
    },
    inputDecimal() {
      if (!this.lastInput.includes(".")) {
        if (this.lastInput === "") {
          this.lastInput += "0.";
        } else {
          this.lastInput += ".";
        }
      }
    },

    //  update memory value
    setOperate(op) {
      this.operator = op;
      this.temp = this.lastInput;
      this.lastInput = "";
    },
    calculate() {
      if (this.operator && this.temp !== "") {
        let result = eval(this.temp + this.operator + this.lastInput);
        this.lastInput = result;
        this.temp = "";
        this.operator = null;
      }
    },
    clear() {
      this.lastInput = "";
      this.temp = "";
      this.operator = null;
    },
    toggleSign() {
      if (this.lastInput) {
        this.lastInput = this.lastInput.startsWith("-")
          ? this.lastInput.slice(1)
          : `-${this.lastInput}`;

        // if(this.lastInput.startsWith("-")){
        //   this.lastInput=this.lastInput.slice(1);
        // }else{
        //   this.lastInput ="-"+ this.lastInput;
        // }
      }
    },
    deleteLast() {
      // delete at the end
      this.lastInput = this.lastInput.slice(0, -1);
    },

    clearMemory() {
      this.memory = 0;
    
       this.lastMemory = "M = 0";
      console.log(this.memory);
    },
    recallMemory() {
      this.lastInput = this.memory.toString();
      this.lastMemory = "M = "+this.memory;
      console.log(this.memory);
    },
    addMemory() {
      this.memory += parseFloat(this.lastInput);

     this.lastMemory = "M = "+this.memory;
      console.log(this.memory);
    },

    substractMemory() {
      this.memory -= parseFloat(this.lastInput);

       this.lastMemory = "M = "+this.memory;
      console.log(this.memory);
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.container {
  margin-top: 10em;
  width: 300px;
  border: 1px solid black;
  padding-top: 20px;
  padding-bottom: 20px;
}
table {
  border-spacing: 7px;
  border-collapse: separate;
}
#screen {
  border: 1px solid black;
  padding: 7px;
  width: 100%;
  height: 4em;
}
#screen_top {
  display: block;
  font-size: 0.8rem;
}
#screen_bottom {
  font-size: 1.8rem;
  display: block;
  text-align: right;
}
#operand2 {
  background-color: skyblue;
}
#operator {
  background-color: rosybrown;
}
.button-row {
  display: flex;
  justify-content: space-between;
}
button {
  width: 45px;
}
.long-btn {
  display: inline-block;
  height: 80px;
}

/* Message panel */
#message_panel {
  width: 300px;
  margin-top: 1em;
  display: none;
  margin-left: auto;
  margin-right: auto;
}
</style>
