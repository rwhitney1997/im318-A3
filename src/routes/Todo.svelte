<!-- To run in Powershell: "npm run dev -- --open" -->

<script>
     let todoItem = $state('');
     let todoList = $state([]); //square brackets for array

function addItem(event) {
     event.preventDefault(); //when enter is clicked the focus remains on the submit form
     if (todoItem == '') { //refuses to take submit when field is empty
          return;
     }
     todoList = [...todoList, {
          text: todoItem,
          done: false
     }];
     todoItem = '';
}
function removeItem(index){
     todoList = todoList.toSpliced(index, 1);
}

function nuke (){
     todoList = [];
}

$effect(() => {
     doneList = todoList.filter((item) => item.done);
})

$inspect(todoList);
</script>

<form class="text-pop-up-top" onsubmit={addItem}>
<input type="text" bind:value={todoItem}>
<button id="inputButton" type="submit">Add Event</button>
</form>

<ul>
     {#each todoList as item, index}
          <li class="slide-in-left">
               <input id="listCheck" type="checkbox" bind:checked={item.done}>
               <span class:done={item.done}>{item.text}</span> <!-- span directive adds class when item is done, specifically - see style below -->
               <button id="listButton" type="button" onclick={() => removeItem(index)} >&#10008</button>
          </li>
     {/each}
</ul>



{#if (todoList.length > 0)}
<button id="listButton" type="button" onclick={nuke}>Clear List</button>
{/if}

<style>
     ul{
          list-style: none;
          text-align: center;
     }
     span.done{
          color: rgb(137, 137, 137);
          text-decoration: line-through;
     }
     /* Styles added here will only effect this component, use style.css for global */
</style>