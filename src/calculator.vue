<script setup>
import { ref, computed, readonly } from "vue";
import { evaluate } from 'mathjs';
let message = ref("");
let font = ref();

const percent = () => {
  if(message.value=="error"){
    message.value="";
  }else{
    message.value = (parseFloat(message.value) / 100).toString();
  }
  
  
};

const clear = () => {
  message.value = "";
};

const append = (number) => {
  if (message.value == "") {
    message.value = message.value + number;

 } else if (
    message.value[message.value.length - 1] == "," ||message.value[message.value.length - 1] == "+"||message.value[message.value.length - 1] == "x"||message.value[message.value.length - 1] == "÷"|| message.value[message.value.length - 1] == "-") {
    message.value = message.value + number;
    
  } else if(message.value=="error"){
    message.value=number;
  }
  else {
    message.value += number;
  }
  
  
};
const sign = () => {
  if (message.value.includes("-")) {
    message.value = message.value.slice(1);
  } else if(message.value=="error"){
    message.value="-";
  }
  else {
    message.value = "-" + message.value;
  }
};
const del = () => {
  if(message.value.includes('Infinity')||message.value.includes('error')){
    message.value="";
  }
  message.value = message.value.slice(0, -1);
};
const comma = () => {
  if (message.value == ""||message.value=="error") {
    message.value = "0,";
  } else if (!message.value.includes(",")) {
    message.value = message.value + ",";
  }
};
const addition = () => {
  if (message.value == ""||message.value=="error") {
    message.value = "0" + "+";
  } else if (
    message.value[message.value.length - 1] == "-" ||
    message.value[message.value.length - 1] == "x" ||
    message.value[message.value.length - 1] == "÷" ||
    message.value[message.value.length - 1] == "+"
  ) {
    message.value = message.value.slice(0, -1);
    message.value = message.value + "+";
  } else {
    message.value = message.value + "+";
  }
};
const substraction = () => {
  if (message.value == ""||message.value=="error") {
    message.value = "0" + "-";
  } else if (
    message.value[message.value.length - 1] == "-" ||
    message.value[message.value.length - 1] == "x" ||
    message.value[message.value.length - 1] == "÷" ||
    message.value[message.value.length - 1] == "+"
  ) {
    message.value = message.value.slice(0, -1);
    message.value = message.value + "-";
  } else {
    message.value = message.value + "-";
  }
};
const multiplication = () => {
  if (message.value == ""||message.value=="error") {
    message.value = "0" + "x";
  } else if (
    message.value[message.value.length - 1] == "-" ||
    message.value[message.value.length - 1] == "x" ||
    message.value[message.value.length - 1] == "÷" ||
    message.value[message.value.length - 1] == "+"
  ) {
    message.value = message.value.slice(0, -1);
    message.value = message.value + "x";
  } else {
    message.value = message.value + "x";
  }
};
const division = () => {
  if (message.value == ""||message.value=="error") {
    message.value = "0" + "÷";
  } else if (
    message.value[message.value.length - 1] == "-" ||
    message.value[message.value.length - 1] == "x" ||
    message.value[message.value.length - 1] == "÷" ||
    message.value[message.value.length - 1] == "+"
  ) {
    message.value = message.value.slice(0, -1);
    message.value = message.value + "÷";
  } else {
    message.value = message.value + "÷";
  }
};
const equal = ()=>{
   try{
     
     if(message.value.includes('÷')||message.value.includes('x')||message.value.includes(',')){
    const result = message.value.replace(/÷/g,'/').replace(/x/g,'*').replace(/,/g,'.');
    message.value=(evaluate(result)).toString();
   }else{
    message.value=(evaluate(message.value)).toString();
  
  }
  if(message.value.includes(Infinity)) throw new Error('error');
  if(message.value.includes(NaN)) throw new Error('error');
   }catch(error){
    message.value='error';
   }
}

  

</script>
<template>
  <body>
     <div class="container">
      <div class="display">
        <input v-model="message" type="text" :style="{fontSize:font+'px'}" @keypress.enter="equal"/>
      </div>
      <div>
        <button style="color:#00cc00" @mousedown="del">⌫</button>
        <button style="color:red" id="clear" @mousedown="clear">C</button>
        <button style="color:#008000" @mousedown="percent">%</button>
        <button style="color:#008000" @mousedown="division">÷</button>
      </div>
      <div>
        <button @mousedown="append(7)">7️</button>
        <button @mousedown="append(8)">8️</button>
        <button @mousedown="append(9)">9️</button>
        <button style="color:#008000" @click="multiplication">X</button>
      </div>
      <div>
        <button @mousedown="append(4)">4️</button>
        <button @mousedown="append(5)">5️</button>
        <button @mousedown="append(6)">6️</button>
        <button  style="color:#008000"  @click="substraction">-</button>
      </div>
      <div>
        <button @mousedown="append(1)">1️</button>
        <button @mousedown="append(2)">2️</button>
        <button @mousedown="append(3)">3️</button>
        <button style="color:#008000"  @click="addition">+</button>
      </div>
      <div>
        <button @mousedown="sign">+/-</button>
        <button @mousedown="append(0)">0️</button>
        <button @mousedown="comma">◞</button>
        <button id="equal-operator" @click="equal">=</button>
      </div>
    </div>
  </body>
</template>
