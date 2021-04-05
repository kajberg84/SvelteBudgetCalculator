<script>



import Title from './Title.svelte';
  export let name = "";
  export let amount = null;
  export let addExpense;
  export let isEditing;
  export let editExpense;
  export let hideForm;

  // import {onMount, onDestroy, beforeUpdate, afterUpdate} from 'svelte'
  // onMount(()=>{
  //   console.log("Form has mounted")
  // })
  // beforeUpdate(()=>{
  //   console.log("Before update")
  // })
  // afterUpdate(()=>{
  //   console.log("After update")
  // })
  // onDestroy(()=>{
  //   console.log("Form is hidden")
  // })

  //Reactivity koll här under. Så fort något ändras i name eller amount så consologas allt
  // $: console.log({name},{amount})

  //A Trouthy value is considered TRUE when encountered in boolean context.
  //Nedan om båda är true kommer isEmpty = true. 
  $: isEmpty = !name || !amount
function handleSubmit() {
  if(isEditing){
    editExpense({name, amount})
  }
  else{
    addExpense({name, amount})
  }
  name = '';
  amount = null;
  hideForm()
}

</script>

<section class="form">
  <Title title="add expense"></Title>
  <!-- on:submit|prevenDefault är i svält istället för att lägga det i funktion -->
  <form class="expense-form" on:submit|preventDefault={handleSubmit}>
    <div class="form-control">
      <label for="name">name</label>
      <input type="text" id="name" bind:value={name}/>
    </div>

    <div class="form-control">
      <label for="amount">amount</label>
      <input type="number" id="amount" bind:value={amount}/>
    </div>

    {#if isEmpty}
    <p class="form-empty">Please fill out all form fields</p>
    {/if}
    <!-- Om isEmpty är false så är knappen true   detta gör att om true lägger man till klassen class:disabled={isEmpty} -->
    <button type="submit" class="btn btn-block " class:disabled={isEmpty} disabled={isEmpty}> 
      {#if isEditing}edit expense{:else}add Expense
      {/if}
    </button>

    <button type="button" class="close-btn" on:click={hideForm}> 
      <i class="fas fa-times"></i>
      close 
    </button>

  </form>
</section>