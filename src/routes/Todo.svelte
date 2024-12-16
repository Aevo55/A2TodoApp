<script>
import {onMount} from 'svelte'

let newItem = $state("")

let listList = $state([{
     items: [{
          text: "",
          done: false,
          edit: false
     }],
     edit: false,
     color: "#FFECA1"
}])

let currList = $state(null)

onMount(() => {
     load()
})

function load(){
     let storedList = localStorage.getItem('storedList')
     if (storedList) {
          listList = (JSON.parse(storedList))
          for(let l of listList){
               edit = false
               for(let i of l.items){
                    
               }
          }
     }
}

function openMode(){
     currList = null
}

function saveAll() {
     return localStorage.setItem('storedList', JSON.stringify(listList))
}

function trashList(){
     let currListIndex = listList.indexOf(currList)
     listList.splice(currListIndex, 1)
     if(listList.length == 0){
     }
     currList = null
}

function newList(){
     listList.push({
          name: "New List",
          items: [],
          color: "#FFECA1"
     })
}

function editListName(index){

}

function openList(index){
     currList = listList[index]
}

function createItem(){
     currList.items.push({
          text: "",
          done: false,
          edit: false
     })
}

function addItem(event) {
     event.preventDefault()
     if (newItem == '') {
          return
     }
     currList.items.push({
          text: newItem,
          done: false,
          edit: false
     })
     newItem = ""
}

function removeItem(index){
     currList.items = currList.items.toSpliced(index, 1)
}

function toggleDone(index){
     currList.items[index].done = !currList.items[index].done
}

function toggleEdit(index){
     currList.items[index].edit = !currList.items[index].edit
}

$inspect(listList)


</script>

<div class="_col">
     <div class="_row buttonBar">
          <button type="button" title="Save" onclick={() => saveAll()}>🖫</button>
          <button type="button" title="Undo" onclick={() => load()}>↻</button>
     </div>
     {#if currList == null}
     <ul>
          {#each listList as item, index}
          <li class="_row">
               <span class="rowText">{item.name}</span>
               <div class="_row">
                    <button class="" type="button" title="Rename" onclick={() => editListName(index)}>✎</button>
                    <button class="" type="button" title="Open" onclick={() => openList(index)}>🗁</button>
                    <button type="button" title="Remove" onclick={() => trashList(index)}>🗑</button>
               </div>
          </li>
          {/each}
          <li><button type="button" title="New List" onclick={() => newList()}>+</button></li>
     </ul> 
     {:else}
     <ul>
          <div class="_row"> 
               <h2>{currList.name}</h2>
               <button type="button" title="Open" onclick={() => openMode()}>⮐</button>
          </div>
         
          {#each currList.items as item, index}
          <li class="_row _jevenly">
               <span class="rowText">{item.text}</span>
               <div class="_row">
                    <button class="" type="button" title="Edit" onclick={() => toggleEdit(index)}>✎</button>
                    <button type="button" title="Done" onclick={() => toggleDone(index)}>☑</button>
                    <button type="button" title="Remove" onclick={() => removeItem(index)}>🗑</button>
               </div>
          </li>
          {/each}
          <li><button type="button" title="New Item" onclick={() => createItem()}>+</button></li>
     </ul>
     {/if}
</div>



<style>

</style>