<template>
  <v-app-bar color="black">
    <v-toolbar-title>Tablero</v-toolbar-title>

    <v-spacer></v-spacer>

    <v-dialog width="800">
      <template v-slot:activator="{ props }">
        <v-btn variant="tonal" v-bind="props" append-icon="mdi-plus"
          >Nuevo ticket</v-btn
        >
      </template>

      <template v-slot:default="{ isActive }">
        <v-card
          title="Nuevo ticket"
          color="black"
          class="py-10"
          style="
            border-radius: 30px;
            box-shadow: 0px 0px 20px 0px rgba(0, 255, 224, 0.75);
            -webkit-box-shadow: 0px 2px 20px 0px rgba(0, 255, 224, 0.75);
            -moz-box-shadow: 0px 2px 20px 0px rgba(0, 255, 224, 0.75);
          "
        >
          <v-card-text>
            <v-text-field
              label="Nombre del ticket"
              variant="outlined"
              v-model="ticket"
            ></v-text-field>
          </v-card-text>

          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn text="Close Dialog" @click="isActive.value = false"></v-btn>
            <v-btn
              variant="tonal"
              text="Agregar ticket"
              @click="
                guardarTicket();
                isActive.value = false;
              "
            ></v-btn>
          </v-card-actions>
        </v-card>
      </template>
    </v-dialog>
  </v-app-bar>
</template>

<script setup>
import ticketsData from "@/mocks/tickets.json";
import { reactive, ref } from "vue";

const tickets = reactive(ticketsData);

const ticket = ref("");

const guardarTicket = () => {
  tickets.pendientes.push(ticket.value);
  console.log(tickets.pendientes);
};
</script>

<style scoped>
.v-toolbar-title {
  color: rgb(0, 221, 184);
  font-size: 1.8rem;
  font-weight: 700;
}

.v-btn {
  color: rgb(0, 221, 184);
}
</style>