<script>
import {onMount} from 'svelte'

let emptyList = []

let newItem = $state("")

let listList = $state([])

let currList = $state(null)

onMount(() => {
     load()
})

function load(){
     let storedList = localStorage.getItem('storedList')
     if (storedList) {
          listList = (JSON.parse(storedList))
          for(let i = 0; i < listList.length; i++){
               listList[i].edit = false
               for(let j = 0; j < listList[i].items.length; j++){
                    listList[i].items[j].edit = false
               }
          }
     }else{
          listList = emptyList
     }
}

function goBack(){
     currList = null
}

function saveAll() {
     return localStorage.setItem('storedList', JSON.stringify(listList))
}

function nuke(){
     localStorage.setItem('storedList', JSON.stringify(emptyList))
     load();
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
          name: "New List " + (listList.length + 1),
          items: [],
          edit: false,
          color: "#FFECA1"
     })
}

function editListName(index){
     let oldEdit = listList[index].edit
     for(let i = 0; i < listList.length; i++){
          listList[i].edit = false;
     }
     listList[index].edit = !oldEdit;
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
          <button class="headerButton" type="button" title="Save" onclick={() => saveAll()}>&#128190;</button>
          <button class="headerButton" type="button" title="Undo" onclick={() => load()}>↻</button>
          <button class="headerButton" type="button" title="Kaboom" onclick={() => nuke()}>💣</button>
     </div>
     {#if currList == null}
     <div class="listHolder">
          {#each listList as item, index}
          <div class="listItem _row _jbetween ">
               {#if item.edit}
               <input class="rowTextInput" type="text" autofocus bind:value={item.name}>
               {:else}
               <div class="rowText">{item.name}</div>
               {/if}
               <div class="_row _jend rowButtons">
                    {#if !item.edit}
                    <button class="rowButton" type="button" title="Rename" onclick={() => editListName(index)}>✎</button>
                    {:else}
                    <button class="rowButton" type="button" title="Rename" onclick={() => editListName(index)}>&#10003;</button>
                    {/if}
                    <button class="rowButton" type="button" title="Open" onclick={() => openList(index)}>&#128193;</button>
                    <button class="rowButton" type="button" title="Remove" onclick={() => trashList(index)}>🗑</button>
               </div>
          </div>
          {/each}
          <div class="_row listItem">
               <button type="button" title="New List" onclick={() => newList()}>+</button>
          </div>
     </div> 
     {:else}
     <div class="listHolder">
          <div class="_row"> 
               <h2>{currList.name}</h2>
               <button type="button" title="Open" onclick={() => goBack()}>⮐</button>
          </div>
         
          {#each currList.items as item, index}
          <div class="_row _jbetween">
               <div class="rowText">{item.text}</div>
               <div class="_row _jend rowButtons">
                    <button class="rowButton" type="button" title="Edit" onclick={() => toggleEdit(index)}>✎</button>
                    <button class="rowButton" type="button" title="Done" onclick={() => toggleDone(index)}>☑</button>
                    <button class="rowButton" type="button" title="Remove" onclick={() => removeItem(index)}>🗑</button>
               </div>
          </div>
          {/each}
          <div class="_row listItem">
               <button type="button" title="New Item" onclick={() => createItem()}>+</button>
          </div>
     </div>
     {/if}
</div>



<style>
     .listHolder{
          width: 80vw;
          background-color: #f9ff3e;
          margin: 20px;
          padding: 10px 20px;
          display: flex;
          flex-direction: column;
          align-items: flex-start;
          box-shadow: 5px 5px;
          border-color: black;
          border-radius: 1px;
          border-width: 1px;
          border-style: solid;
          position: relative;
     }

     .listItem{
          position: relative;
          height: 50px;
          align-items: center;
          width: 100%;
     }
     
     .listItem:not(:first-child)::before {
          position: absolute;
          display: block;
          content: "";
          width: calc(80vw + 10px);
          top: 0px;
          left: -10px;
          height: 1.5px;
          background-color: grey;
     }

     .rowText{
          margin-left: 20px;
     }

     button{

          margin: 0px 0.3vw;
     }

     .rowButtons{
          height: 70%;
     }
</style>