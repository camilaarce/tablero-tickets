<template>
  <div class="container">
    <h1 class="text-center mb-5">Tickets</h1>
    <div class="fila">
      <div class="columna">
        <div class="tablero" @dragover="allowDrop" @drop="dropHandler">
          <h2>Pendientes</h2>
          <div
            v-for="(pendiente, index) in pendientes"
            :key="index"
            :draggable="true"
            @dragstart="dragStartHandler(pendiente, 'pendientes')"
          >
            <div
              class="ticket"
              style="display: flex; align-items: center; padding-left: 3%"
            >
              <p>{{ pendiente }}</p>
            </div>
          </div>
        </div>
      </div>
      <div class="columna">
        <div class="tablero" @dragover="allowDrop" @drop="dropHandler">
          <h2>En progreso</h2>
          <div
            v-for="(progreso, index) in progresos"
            :key="index"
            :draggable="true"
            @dragstart="dragStartHandler(progreso, 'progresos')"
          >
            <div
              class="ticket"
              style="display: flex; align-items: center; padding-left: 3%"
            >
              <p>{{ progreso }}</p>
            </div>
          </div>
        </div>
      </div>
      <div class="columna">
        <div class="tablero" @dragover="allowDrop" @drop="dropHandler">
          <h2>Listo</h2>
          <div
            v-for="(listo, index) in listos"
            :key="index"
            :draggable="true"
            @dragstart="dragStartHandler(listo, 'listos')"
          >
            <div
              class="ticket"
              style="display: flex; align-items: center; padding-left: 3%"
            >
              <p>{{ listo }}</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";

const pendientes = ref([
  "pendiente 1",
  "pendiente 2",
  "pendiente 3",
  "pendiente 4",
]);
const progresos = ref([
  "progreso 1",
  "progreso 2",
  "progreso 3",
  "progreso 4",
  "progreso 5",
]);
const listos = ref(["listo 1", "listo 2"]);

let draggedItem = null;
let sourceTablero = null;

const allowDrop = (event) => {
  event.preventDefault();
};

const dragStartHandler = (item, tablero) => {
  draggedItem = item;
  sourceTablero = tablero;
};

const dropHandler = (event) => {
  event.preventDefault();
  if (draggedItem) {
    switch (sourceTablero) {
      case "pendientes":
        pendientes.value = pendientes.value.filter(
          (item) => item !== draggedItem
        );
        break;
      case "progresos":
        progresos.value = progresos.value.filter(
          (item) => item !== draggedItem
        );
        break;
      case "listos":
        listos.value = listos.value.filter((item) => item !== draggedItem);
        break;
    }

    // Agregar el ítem al nuevo tablero
    const targetTablero = event.target.closest(".tablero");
    if (targetTablero) {
      const targetTableroName = targetTablero
        .querySelector("h2")
        .innerText.toLowerCase();
      switch (targetTableroName) {
        case "pendientes":
          pendientes.value.push(draggedItem);
          break;
        case "en progreso":
          progresos.value.push(draggedItem);
          console.log("progrso");
          break;
        case "listo":
          listos.value.push(draggedItem);
          break;
      }
    }

    // Limpiar las variables después de soltar
    draggedItem = null;
    sourceTablero = null;
  }
};
</script>


<style>
@import url("https://fonts.googleapis.com/css2?family=Comfortaa:wght@300&display=swap");

* {
  margin: 0px;
  padding: 0px;
  box-sizing: border-box;
  font-family: "Comfortaa", sans-serif;
}

.container {
  padding: 3%;
  height: 100vh;
  width: 100%;
  background-color: #000;
}

.fila {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1%;
}

h1,
h2,
p {
  color: #fff;
}

.tablero {
  height: 80vh;
  border: 2px solid white;
  padding: 3%;
  border-radius: 20px;
  transition: all 1s ease;
  background-color: #0c0c0c;
}

.tablero:hover {
  border: 2px solid rgb(0, 221, 184);
  box-shadow: 0px 4px 44px 0px rgba(0, 255, 224, 0.75);
  -webkit-box-shadow: 0px 4px 44px 0px rgba(0, 255, 224, 0.75);
  -moz-box-shadow: 0px 4px 44px 0px rgba(0, 255, 224, 0.75);
}

.ticket {
  height: 5vh;
  width: 100%;
  border: 1px solid white;
  margin: 2% 0;
  border-radius: 10px;
  background-color: #000;
  transition: all 1s ease;
}

.ticket:hover {
  background-color: #123541;
}

@media only screen and (max-width: 767px) {
  .fila {
    display: flex;
    overflow-x: auto;
  }

  .columna {
    flex: 0 0 auto;
    width: 100%;
    margin-right: 10px;
  }
}
</style>