<template>
  <div>
    <span>Add new todo:</span>
    <input type="text" v-model="todoListItem" @keypress.enter="addTodoItem" />
    <h2 v-if="todoList.length">Todos</h2>
    <ul @drop="onDrop" @dragover="onDragOver" @dragstart="onDragStart">
      <li v-for="(item,index) of todoList" :key="index" :id="index" draggable="true">
        {{item}}
        <button @click="removeTodoItem(index)">remove</button>
      </li>
    </ul>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';

@Component
export default class Todolist extends Vue {
  public todoList: string[] = []; /* storing all of our todos */
  public todoListItem: string = ''; /* input v-model property */
  public dragableElementId: number = -1; /* setup default value */

  public addTodoItem(): void {
    this.todoList.push(this.todoListItem); /* adding new item to the list */
    this.todoListItem = ''; /* clearing the input field */
  }

  public removeTodoItem(index: number): void {
    this.todoList.splice(index, 1); /* remove the unwanted item */
  }

  public onDragStart(event: any) {
    this.dragableElementId = event.target.id;
  }

  /* events fired on the drop targets */
  public onDragOver(event: any) {
    event.preventDefault(); /* prevent default to allow drop  */
  }

  public onDrop(event: any) {
    event.preventDefault();
    if (event.target.id !== this.dragableElementId) {
      this.swapArrayItems(event.target.id, this.dragableElementId);
    }
    this.dragableElementId = -1; /* reset to the default value */
  }



  /* swap items by passing positions */
  private swapArrayItems(source: number, destination: number): void {
    const tmp: string[] = this.todoList.splice( source, 1, this.todoList[destination] );
    this.todoList.splice(destination, 1, tmp[0]);
  }
}
</script>


<style scoped>
ul {
  width: fit-content;
  margin: 0 auto;
}
li {
  padding: 5px;
}
</style>