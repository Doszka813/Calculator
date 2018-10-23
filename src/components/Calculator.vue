<template>
  <div class="calc">
    <div id="display">{{current || 0}}</div>
    <div v-on:click="clear" class="btn">C</div>
    <div v-on:click="sign" class="btn">+/-</div>
    <div v-on:click="percent" class="btn"><i class="fas fa-percentage"></i></div>
    <div v-on:click="divide" class="btn operator"><i class="fas fa-divide"></i></div>
    <div v-on:click="append('7')" class="btn">7</div>
    <div v-on:click="append('8')" class="btn">8</div>
    <div v-on:click="append('9')" class="btn">9</div>
    <div v-on:click="multiply" class="btn operator"><i class="fas fa-times"></i></div>
    <div v-on:click="append('4')" class="btn">4</div>
    <div v-on:click="append('5')" class="btn">5</div>
    <div v-on:click="append('6')" class="btn">6</div>
    <div v-on:click="minus" class="btn operator"><i class="fas fa-minus"></i></div>
    <div v-on:click="append('1')" class="btn">1</div>
    <div v-on:click="append('2')" class="btn">2</div>
    <div v-on:click="append('3')" class="btn">3</div>
    <div v-on:click="add" class="btn operator"><i class="fas fa-plus"></i></div>
    <div v-on:click="append('0')" class="btn" id="zero">0</div>
    <div v-on:click="dot"  class="btn">.</div>
    <div v-on:click="equal" class="btn operator"><i class="fas fa-equals"></i></div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      previous: null,
      current: '',
      operator: null,
      operatorActivated: false
    }
  },
  methods: {
    clear() {
      this.current = ''
    },
    sign() {
      this.current = this.current.charAt(0) === '-' ?
        this.current.slice(1) : `-${this.current}`;
    },
    percent() {
      this.current = `${parseFloat(this.current)/100}`;
    },
    append(num) {
      if(this.operatorActivated) {
        this.current = '';
        this.operatorActivated = false;
      }
      this.current = `${this.current}${num}`;
    },
    dot() {
      this.current.includes('.') ? this.current : this.append('.')
    },
    setPrevious() {
      this.previous = this.current;
      this.operatorActivated = true;
    },
    divide() {
      this.operator = (a, b) => a / b;
      this.setPrevious();
    },
    multiply() {
      this.operator = (a, b) => a * b;
      this.setPrevious();
    },
    minus() {
      this.operator = (a, b) => a - b;
      this.setPrevious();
    },
    add() {
      this.operator = (a, b) => a + b;
      this.setPrevious();
    },
    equal() {
      this.current = `${this.operator(
        parseFloat(this.previous), 
        parseFloat(this.current))}`;
      this.previous = null;
    }
  }
}
</script>

<style scoped>

.calc {
  font-size: 40px;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: minmax(50px, auto);
  margin: 0 auto;
  width: 400px;
  border: #333;
}

#display {
  grid-column: 1/5;
  background-color: #333;
  color: white;
  padding: 10px 15px;
  font-size: 30px;
}

#zero {
  grid-column: 1/3;
}

.btn {
  background-color: #eee;
  border: 1px solid #999;
  padding: 5px;
}

.btn:hover {background-color: rgba(184, 181, 181, 0.582)}

.btn:active {
  background-color: orange;
  box-shadow: 0 5px #666;
  transform: translateY(2px)
}

.operator {
  background-color: orange;
  color: white;
}
</style>
