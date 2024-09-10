<script setup>
import { ref } from "vue";
let list = ref(["raz", "dwa", "trzy"]);
let element = ref("");

function addElement() {
  if (element.value == "") return;
  list.value.push(element.value);
  element.value = "";
}

function removeElement(index) {
  console.log(index);
  list.value.splice(index, 1);
}
</script>

<template>
  <div class="column is-one-third">
    <div class="box">
      <h1 class="title">Elementy listy</h1>
      <p class="notification is-primary" v-for="(element, index) in list"> {{ element }}
        <button class="delete" v-on:click="removeElement(index)"></button>
      </p>
      <div class="field is-grouped">
        <input type="text" class="input" v-model="element" v-on:keyup.enter="addElement" placeholder="Wprowadź element" autofocus/>
        <button class="button is-link" v-on:click="addElement">Dodaj</button>
      </div>
    </div>
  </div>
</template>

<style scoped>

</style>
