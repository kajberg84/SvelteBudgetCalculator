<script>
	import { setContext, onMount, afterUpdate } from 'svelte'
	// Components
	import Navbar from './Navbar.svelte'
	import ExpensesList from './ExpensesList.svelte'
	import Totals from './Totals.svelte'
	import ExpenseForm from './ExpenseForm.svelte'
	import Modal from './Modal.svelte'
	// Data
	// import expensesData from './expenses.js'
	// variables
	let expenses = []

	// set editing variables
	let setName = ''
	let setAmount = null
	let setId = null

	//toggle form variables
	let isFormOpen = false;

	// reactive
$: isEditing = setId ? true : false;//if setId = true retun true or else false
$: total = expenses.reduce((acc,curr)=>{
	return(acc += curr.amount)
},0)
	//functions

const showForm = () => {
	isFormOpen = true;
}

const hideForm = () => {
	isFormOpen = false;
	setName = '';
	setAmount = null;
	setId = null;
}

	const removeExpense = (id) => {
		expenses = expenses.filter(item => item.id !== id)
	}

 const clearExpenses = () => {
	 expenses=[]
 }

const addExpense = ({name, amount}) => {
 console.log({name, amount})
 let expense = {
	 id:Math.random()* Date.now(), //för att slumpa ett id.
	 name,
	 amount
 }
  // här gör vi om arrayen expenses och lägger den nya expense först och sen kopierar den gamla.
	//vill man ha expense länst ner så [...expenses, expense]
 expenses = [expense, ...expenses]
}

const setModifiedExpense = (id) => {
	let expense = expenses.find(item => item.id === id) // return if item.id  = id
	setId = expense.id;
	setName = expense.name;
	setAmount = expense.amount; 
	showForm()
}

const editExpense = ({name, amount}) => {
	expenses = expenses.map(item => {
		return item.id === setId? {...item,name,amount}:item
	})
	setId = null;
	setAmount=null;
	setName = "";

}

//context
setContext("remove", removeExpense)
setContext("modify", setModifiedExpense)
//local storage 
//varje gång den körs så skrivs localstorage över.
const setLocalStorage = () => {
	localStorage.setItem("expenses", JSON.stringify(expenses))
}

onMount(()=>{
	expenses = localStorage.getItem("expenses")?
	JSON.parse(localStorage.getItem("expenses"))
	:[]
})

//Eftersom detta körs varje gång man editerar så bör man ej ha tunga funktioner i afterupdate.
afterUpdate(()=>{
	console.log("after Update")
	setLocalStorage()
})

</script>

<Navbar {showForm}/>

<main class="content">
{#if isFormOpen}
<Modal>
	<ExpenseForm 
	{addExpense} 
	name={setName} 
	amount={setAmount} 
	{isEditing} 
	{editExpense}
	{hideForm}
	/>
</Modal>
	{/if}
	<Totals title="Total expenses" {total} />
	<ExpensesList {expenses}/>
	<button type="button" class="btn btn-primary btn-block" on:click={clearExpenses}>
		clear expenses
	</button>
</main>
