<script>
let todoItem = $state('');
let todoList = $state([]);
let prio = $state('');

function addItem(event) {
     event.preventDefault();
     if (todoItem == '') {
          return;
     }
     todoList.push({
          text: todoItem,
          priority: 0,
          done: false
     });
     todoList.sort(function(a, b){return a.priority - b.priority})
     todoItem = '';
}

function changePrio(event){
     todoList.sort(function(a, b){return a.priority - b.priority})
}

$inspect(todoList);


</script>
<form onsubmit={addItem}>
     <input type="text" bind:value={todoItem}>
     <button type="submit">Add</button>
</form>

<ul>
     {#each todoList as item}
          <li>
               <input type="checkbox" bind:checked={item.done}>
               <input type="number" bind:value={item.priority} onchange={changePrio}>
               {item.text} 
               
          </li>
     {/each}
</ul>


<style>

</style>