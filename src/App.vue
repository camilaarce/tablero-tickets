<template>
  <v-app>
    <div class="navbar">
      <Navbar />
    </div>
    <v-main style="margin-top: -34px">
      <div class="container">
        <h1 class="text-center mb-5">Tickets</h1>
        <div class="fila">
          <div class="columna">
            <div class="tablero" @dragover="allowDrop" @drop="dropHandler">
              <h2>Pendientes</h2>
              <div
                v-for="(pendiente, index) in tickets.pendientes"
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
                v-for="(progreso, index) in tickets.progresos"
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
                v-for="(listo, index) in tickets.listos"
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
    </v-main>
  </v-app>
</template>

<script setup>
import Navbar from "@/components/Navbar.vue";
import ticketsData from "@/mocks/tickets.json";
import { onMounted, reactive } from "vue";

onMounted(() => {
  window.onload = function () {
    //select the thing we wanna drag
    var ticket = document.querySelector(".ticket");
    const targetTicketName = ticket.querySelector("p").innerText.toLowerCase();
    ticket.addEventListener("touchmove", function (ev) {
      var tablero = this.closest(".tablero");
      const targetTableroName = tablero
        .querySelector("h2")
        .innerText.toLowerCase();
      switch (targetTableroName) {
        case "pendientes":
          tickets.pendientes = tickets.pendientes.filter(
            (item) => item !== targetTicketName
          );
          break;
        case "en progreso":
          tickets.progresos = tickets.progresos.filter(
            (item) => item !== targetTicketName
          );
          break;
        case "listo":
          tickets.listos = tickets.listos.filter(
            (item) => item !== targetTicketName
          );
          break;
      }
    });
    ticket.addEventListener("touchend", function (ev) {
      var tablero = this.closest(".tablero");
      var tablero = this.closest(".tablero");
      const targetTableroName = tablero
        .querySelector("h2")
        .innerText.toLowerCase();
      switch (targetTableroName) {
        case "pendientes":
          tickets.pendientes.push(targetTicketName);
          break;
        case "en progreso":
          tickets.progresos.push(targetTicketName);
          break;
        case "listo":
          tickets.listos.push(targetTicketName);
          break;
      }
    });
  };
});

const tickets = reactive(ticketsData);

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
        tickets.pendientes = tickets.pendientes.filter(
          (item) => item !== draggedItem
        );
        break;
      case "progresos":
        tickets.progresos = tickets.progresos.filter(
          (item) => item !== draggedItem
        );
        break;
      case "listos":
        tickets.listos = tickets.listos.filter((item) => item !== draggedItem);
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
          tickets.pendientes.push(draggedItem);
          break;
        case "en progreso":
          tickets.progresos.push(draggedItem);
          break;
        case "listo":
          tickets.listos.push(draggedItem);
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
  height: 100%;
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
  height: 70vh;
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
    width: 80%;
    margin-right: 10px;
  }
}
</style>