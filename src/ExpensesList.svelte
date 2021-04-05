<script>
import SectionTitle from './Title.svelte';
import Expense from './Expense.svelte';
import { fly } from 'svelte/transition';
export let expenses =  [];

</script>

<style>
  h2{
    text-transform: capitalize;
  }
</style>


<section>
<SectionTitle title="expense list" />
<ul>
  {#each expenses as expense, index}
  <!-- Om man ej skickar med något på on:delete. alltså on:delete= {} så skickar man dispatch forward -->
  <div transition:fly={{x:200, delay: index*300}}>
    <Expense {...expense}/>
  </div>
  {:else}
  <h2>No expenses added to the list</h2>
  {/each}
</ul>
</section>