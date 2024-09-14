<script setup>
import { ref, useTemplateRef, onMounted, onUpdated } from "vue";
let element = ref("");
let list = ref(null);
const elementInput = useTemplateRef("elementInput");
const ELEMENTS_URL = "http://localhost:8080/elements";

onMounted(() => {
  getElements();
  setElementInputFocus();
});

onUpdated(() => {
  setElementInputFocus();
});

function setElementInputFocus() {
  elementInput.value.focus();
}

function getElements() {
  fetch(ELEMENTS_URL)
      .then((response) => response.json())
      .then((data) => {
        list.value = data
        console.log("Getting elements: " + list.value);
      })
      .catch(function (error) {
        console.log("Getting elements error: " + error);
      });
}

function addElement() {
  if (element.value == "") return;
  fetch(ELEMENTS_URL, {
    method: "POST",
    body: element.value,
  }).then(function (response) {
    if (response.ok) {
      console.log("Adding element: " + element.value);
      element.value = "";
      getElements();
    }
  }).catch(function (error) {
    console.log("Adding element error: " + error);
  });
}

function removeElement(index) {
  fetch(ELEMENTS_URL + index,  {
    method: "DELETE",
  }).then(function (response) {
    if (response.ok) {
      console.log("Deleting element: " + list.value[index]);
      getElements();
    }
  }).catch(function (error) {
    console.log("Deleting element error: " + error);
  });
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
        <input type="text" class="input" v-model="element" v-on:keyup.enter="addElement" placeholder="Wprowadź element" ref="elementInput"/>
        <button class="button is-link" v-on:click="addElement">Dodaj</button>
      </div>
    </div>
  </div>
</template>

<style scoped>

</style>
