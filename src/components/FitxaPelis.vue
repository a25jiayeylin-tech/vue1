<script setup>
import { ref } from 'vue'
import { info } from '@/services/communicationManager'

defineProps({
  element: {
    type: Object,
    required: true
  }
})

const peli = ref(null) 

async function verInfo(id) {
    peli.value = null 
    peli.value = await info(id)
}

</script>

<template>
  <v-card class="pa-4 h-100 d-flex flex-column justify-space-between" elevation="2"  color="#0F172A" border>

      <v-card-title class="text-h6 font-weight-bold px-0 text-wrap text-left">
        {{ element.Title }}
      </v-card-title>

      <v-img :src="element.Poster" alt=""  max-height="250" cover class="rounded-lg my-3"></v-img>
      
      <v-card-subtitle >
        <v-icon icon="mdi-calendar" size="small" class="mr-1"></v-icon>
        {{ element.Year }}
      </v-card-subtitle>
      
      <br>

      <v-dialog max-width="500">
          <template v-slot:activator="{ props: activatorProps }">
              <v-btn
              v-bind="activatorProps"
              text="Más info."
              variant="flat"
              color='#1867C0'
              @click="verInfo(element.imdbID)"
              ></v-btn>
          </template>

          <template v-slot:default="{ isActive }">
              <v-card v-if="peli" class="c2">
                  <v-card-title class="text-h5 font-weight-bold pt-2 px-2 ">
                      {{ peli.Title }}
                  </v-card-title>
                  <v-img :src="element.Poster" alt=""  max-height="250"   class="rounded-lg my-3" ></v-img>
                  <v-card-text class="py-2 px-2 text-body-1">
                      <p><b>Género:</b> {{ peli.Genre }}</p>
                      <p><b>Director:</b> {{ peli.Director }}</p>
                      <p><b>Escritores:</b> {{ peli.Writer }}</p>
                      <p><b>Actores:</b> {{ peli.Actors }}</p>
                  </v-card-text>               
                      <v-btn
                          text="Cerrar"
                          @click="isActive.value = false"
                          color='#4FC3F7'
                      ></v-btn>
              </v-card>
          </template>
      </v-dialog>
  </v-card>
</template>
<style>
.v-application__wrap {
 background: linear-gradient(135deg, #2e39d1 50%, #5e88ca 50%);
}
.c2 {
 background: linear-gradient(135deg, #1e3a8a 0%, #3b82f6 100%);
 border-radius: 16px;
}
</style>
