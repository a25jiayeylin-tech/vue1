<script setup>
import { ref } from 'vue'
import { buscar } from '@/services/communicationManager'
import FitxaPelis from './FitxaPelis.vue'

const cerca = ref('')
const resultats = ref([])

const cercaFeta = ref(false) 


async function ferCerca() {
    if (!cerca.value.trim()) return
    cercaFeta.value = false
    resultats.value = await buscar(cerca.value) || []
    cercaFeta.value = true
    
    
}

const limpiar = () => {
    cerca.value = ''
    resultats.value = []
    cercaFeta.value = false 
}

</script>

<template >
   <v-container>
       <h1 align="center">Buscador de Películes</h1>
        <v-row align="center">
            <v-text-field
                v-model="cerca"
                label="Què vols cercar?"
                clearable
                @keydown.enter="ferCerca"
            ></v-text-field>
            <v-btn
                @click="ferCerca" 
            >
                Cercar
            </v-btn>
            <v-btn
                @click="limpiar"
            >
                Netejar
            </v-btn>
       </v-row>
        <v-alert  v-if="cercaFeta && !resultats.length" type="warning" class="ma-4">
        No s'ha trobat cap resultat per a "{{ cerca }}".
        </v-alert>
       <v-row>
           <v-col
               v-for="element in resultats" :key="element.imdbID"
               cols="12"
               sm="6"
               md="4"
               align="center"
           >
               <FitxaPelis :element="element" />
           </v-col>
       </v-row>
   </v-container>
</template>
