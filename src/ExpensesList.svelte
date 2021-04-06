<script>
import SectionTitle from './Title.svelte';
import Expense from './Expense.svelte';
import { fly } from 'svelte/transition';
import { flip } from 'svelte/animate';
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
    <!-- Om man ej skickar med något på on:delete. alltså on:delete= {} så skickar man dispatch forward -->
  {#each expenses as expense, index (expense.id)}
  <div 
  in:fly={{x:200, delay: index*300}} 
  out:fly={{x: -200 }}
  animate:flip>
    <Expense {...expense}/>
  </div>
  {:else}
  <h2>No expenses added to the list</h2>
  {/each}
</ul>
</section>