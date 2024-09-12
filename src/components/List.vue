<script setup>
import { ref } from "vue";
let element = ref("");
let list = ref(null);


fetch("http://localhost:8080/elements")
    .then((response) => response.json())
    .then((data) => {list.value = data});


function addElement() {
  if (element.value == "") return;
  fetch("http://localhost:8080/elements", {
    method: "POST",
    body: element.value,
  });
  list.value.push(element.value);
  element.value = "";
}

function removeElement(index) {
  fetch("http://localhost:8080/elements/" + index,  {
    method: "DELETE",
  });
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
