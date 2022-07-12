
//ts
<script setup lang="ts">
import { ref } from 'vue';

//切记不要忘记是响应式数据
let result = ref('');
let resultcopy = ref('');
let lastValue: string = '';

const functionBtn: Array<string> = ['AC', 'Del', '+/-', '/', '*', '-', '+', '%', '.', '='];
// const numBtn: Array<number> = [1, 2, 3, 4, 5, 6, 7, 8, 9, 0];


function clickBtn(event: MouseEvent) {
  const target: HTMLElement = event.target as HTMLElement
  const value: string = target.innerText;
  const className: string = target.className;
  if (className !== '') {
    if(className==='res'){
      return;
    }
    if (functionBtn.indexOf(value) !== -1 && result.value === '') {
      console.log('while result is empty ,you cann\'t use functionBtn');
    } else if (functionBtn.indexOf(lastValue) !== -1 && functionBtn.indexOf(value) !== -1 && lastValue !== '=' && lastValue !== '+/-'&&lastValue!=='Del') {
      console.log('you cann\'t use functionBtn agian');
    }else if(functionBtn.indexOf(value)===-1&&result.value==='0'){
      console.log('cannot input number')
    }
    else {
      switch (value) {
        case 'AC':
          result.value = '';
          break;
        case 'Del':
          result.value = result.value.slice(0, result.value.length - 1);
          break;
        case '+/-':
          result.value = '-(' + result.value + ')';
          break;
        case '=':
          resultcopy.value = result.value;
          let temp:string=result.value;
          result.value = eval(temp).toString() as string;
          break;
        default:
          result.value += value;
          break;
      }
    }
  }
  lastValue = value;
  console.log(result.value)
}

</script>

//html
<template>
  <div class="calculator">
    <table @click="clickBtn($event)">
      <caption>
        Calculator
      </caption>
      <tr>
        <td class="res" colspan="4">{{ result !== '' ? result : 'result' }}</td>
      </tr>
      <tr>
        <td class="res" colspan="4">{{ resultcopy !== '' ? resultcopy : 'history' }}</td>
      </tr>
      <tr>
        <td class="function">AC</td>
        <td class="function">Del</td>
        <td class="function">+/-</td>
        <td class="function">/</td>
      </tr>
      <tr>
        <td class="num">7</td>
        <td class="num">8</td>
        <td class="num">9</td>
        <td class="function">*</td>
      </tr>
      <tr>
        <td class="num">4</td>
        <td class="num">5</td>
        <td class="num">6</td>
        <td class="function">-</td>
      </tr>
      <tr>
        <td class="num">1</td>
        <td class="num">2</td>
        <td class="num">3</td>
        <td class="function">+</td>
      </tr>
      <tr>
        <td class="function">%</td>
        <td class="num">0</td>
        <td class="function">.</td>
        <td class="function">=</td>
      </tr>
    </table>
  </div>

</template>


//css
<style scoped>
table {
  border-spacing: 1rem;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

caption {
  font-size: 3.5rem;
}


table tr:nth-child(4) {
  color: blue;
}

td {
  /* 字体大小 */
  font-size: 5rem;
  /* 单元格宽高 */
  width: 6rem;
  height: 6rem;
  /* 内容居中 */
  text-align: center;
  /* 边框 */
  border: 2px solid black;
}

tr td:nth-child(4) {
  color: red;
}

.res {
  font-size: 1.8rem;
  height: 4rem;
  border: 1px solid red;
}

.function {
  font-size: 3rem;
}
</style>
