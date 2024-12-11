<!-- To run in Powershell: "npm run dev -- --open" -->

<script>
     import {onMount} from 'svelte';
     import { fly } from 'svelte/transition';
     import { Confetti } from "svelte-confetti";
     import { M } from 'svelte-motion'
     let todoItem = $state('');
     let todoList = $state([]); //square brackets for array
     let storedList;

     onMount (() => {
          storedList = localStorage.getItem('storedList');
          if (storedList) {
               todoList = (JSON.parse(storedList));
          }
     })

function updateList() {
     return storedList = localStorage.setItem('storedList', JSON.stringify
     (todoList));
}

function addItem(event) {
     event.preventDefault(); //when enter is clicked the focus remains on the submit form
     if (todoItem == '') { //refuses to take submit when field is empty
          return;
     }
     todoList = [...todoList, {
          text: todoItem,
          done: false
     }];
     updateList();
     todoItem = '';
}
function removeItem(index){
     todoList = todoList.toSpliced(index, 1);
     updateList();
}

function nuke (){
     todoList = [];
     localStorage.clear();
}


$inspect(todoList);
</script>

<form class="text-pop-up-top" onsubmit={addItem}>
<input type="text" bind:value={todoItem}> 

<button id="inputButton" type="submit">Add Event</button>
</form>

<ul>
     {#each todoList as item, index}
          <li  in:fly={{ x: -200, duration: 500 }} out:fly={{ x: 200, duration: 500 }}>
               <span class="relative">
               <input id="listCheck" type="checkbox" bind:checked={item.done} onchange={updateList} > 
               {#if (item.done)}
               <span class="confetti">
                    <Confetti y={[-0.5, 0.5]} x={[-0.5, 0.5]} colorRange={[0, 255]} amount=50 fallDistance=100px duration=2000 size=10 />
               </span>
               {/if}
               </span> 
               <span class:done={item.done}>{item.text}<input type="time" name="timestamp" class="inputTime"></span> <!-- span directive adds class when item is done, specifically - see style below -->
               <button id="listButton" type="button" onclick={() => removeItem(index)}>&#10008</button>
          </li>
     {/each}
</ul>

{#if (todoList.length > 0)}
<button class="clearButton" type="button" onclick={nuke}>Clear List</button>
{/if}

<style>
     ul{
          list-style: none;
          text-align: center;
     }
     span.done{
          color: rgb(67, 67, 67);
          text-decoration: line-through;
     }
     
     /* Styles added here will only effect this component, use style.css for global */
</style>