<script>
	import ItemOne from '../components/itemOne.svelte';
	import ItemTwo from '../components/itemTwo.svelte';
    import ItemThree from '../components/itemThree.svelte';
    import List from '../components/List.svelte';
    import Button from '../components/Button.svelte';

    let active = false;

    import { getRandomNumber } from './utils.js';

    
    let response;
    let id;
    let name;

    const fetchData = async () => {
        const url = 'http://localhost:5028/swagger/index.html';
        const response = await fetch(url);
        const data = await response.json();

        this.id = data.id;
        this.name = data.name;
    };

    let newItem = '';
	
    let todoList = [{text: 'Write my first post', status: true},
                    {text: 'Bliver mester i svelte', status: false}]
	
	function addToList() {
		todoList = [...todoList, {text: newItem, status: false}];
		newItem = '';
	}
	
	function removeFromList(index) {
		todoList.splice(index, 1)
		todoList = todoList;
    }

let promise = getRandomNumber();

function handleClick() {
    promise = getRandomNumber();
}

let display_number = "";
	const select = (num) => () => (display_number += num);
	const clear = () => (display_number = "");

	let operand;
	let result;
    let operator;
let operators = ["+", "-", "*", "/"];

	function operation(sign) {
    	operand = Number(display_number);
        operator = sign;
    	display_number = "";
  }

function equals() {
    display_number = Number(display_number);
    if (operator === "+") {
      result = operand + display_number;
    } else if (operator === "-") {
      result = operand - display_number;
    } else if (operator === "*") {
      result = operand * display_number;
    } else if (operator === "/") {
      result = operand / display_number;
    }
display_number = result.toString()
}

	


	
	const items = [
		{ value: 'I am red!', component: ItemOne },
		{ value: 'I am blue!', component: ItemTwo },
        { value: 'I am green!', component: ItemThree}
	];
</script>

<h1>
	Svelte list
</h1>
<List {items} />
<button class:active={active} on:click="{() => active = !active}">I can change color!!</button>
<Button/>

<button on:click={handleClick}>
	generate random number
</button>

{#await promise}
	<p>...waiting</p>
{:then number}
	<p>The number is {number}</p>
{:catch error}
	<p style="color: red">{error.message}</p>
{/await}

<div class="calculator">
<div class="display">{display_number}</div>
    <div class="buttons">
        <button on:click={select(7)}>7</button>
        <button on:click={select(8)}>8</button>
        <button on:click={select(9)}>9</button>
        <button on:click={() => operation(operators[2])} class="operator">*</button>
        <button on:click={select(4)}>4</button>
        <button on:click={select(5)}>5</button>
        <button on:click={select(6)}>6</button>
        <button on:click={() => operation(operators[1])} class="operator">-</button>
        <button on:click={select(3)}>3</button>
        <button on:click={select(2)}>2</button>
        <button on:click={select(1)}>1</button>
        <button on:click={() => operation(operators[0])} class="operator">+</button>
        <button on:click={select(0)}>0</button>
        <button>.</button>
        <button on:click={clear} class="clear">C</button>
        <button on:click={() => operation(operators[3])} class="operator">/</button>
        <button on:click={equals} class="equals">=</button>


    </div>


</div>
<br/>

<input bind:value={newItem} type="text" placeholder="new todo item..">
<button on:click={addToList}>Add</button>

<br/>
{#each todoList as item, index}
<input bind:checked={item.status} type="checkbox">
<span class:checked={item.status}>{item.text}</span>
<span on:click={() => removeFromList(index)}>✅</span>
<br/>  
{:else}
<p>No tasks today! 🥳</p>
{/each}

<br/>
<button on:click={fetchData}>Fetch Data</button>
<p>Id: {id}</p>
<p>Name: {name}</p>




<style>
	.active {
		background-color: blue;
	}

    .calculator {
	display: inline-block;
	border: 1px solid #ccc;
	padding: 10px;
	border-radius: 5px;
	box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

.display {
	font-size: 24px;
	border: 1px solid #ccc;
	height: 24px;
	padding: 10px;
	margin-bottom: 10px;
	width: 300px;
	text-align: right;
}

.buttons {
	display: grid;
	grid-template-columns: repeat(4, 75px);
	gap: 5px;
}

button {
	padding: 10px;
	font-size: 18px;
	border: none;
	cursor: pointer;
	border-radius: 5px;
	font-family: Arial, sans-serif;
	text-align: center;
}
.operator{
	border: 0px solid #A46D19;
	background: #F90;
}
.clear{
	border: 0px solid #000;
	background: red;
}
.equals{
	border: 0px solid #000;
	background: #245BE9;
	width: 320px
}
.operator:hover{
	background: #554a09;
}
.clear:hover {
	background: #FFC0CB ;
}
.equals:hover {
	background: #ADD8E6; 
}
.checked {
        text-decoration: line-through;
    }
</style>