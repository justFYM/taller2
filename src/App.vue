<script setup>
import { ref, computed } from 'vue'
const jugador = ref('X') //Variable que permite identificar al usuario que está en el turno.
const tablero = ref([ //Tablero inicial, si se inicia el juego parte sin datos//
  ['', '', ''],
  ['', '', ''],
  ['', '', '']
])
const saberGanador = (tablero) => {
  const jugadasGanadoras = [ //Aquí van las líneas que si se completan, se acaba el juego, en otras palabras, hay un ganador.
    [1, 4, 7],
    [6, 7, 8],
    [0, 3, 6],
    [2, 5, 8],
    [0, 1, 2],
    [3, 4, 5],
    [0, 4, 8],
    [2, 4, 6]]

  for (let i = 0; i < jugadasGanadoras.length; i++) {
    const [a, b, c] = jugadasGanadoras[i]
    if (tablero[a] && tablero[a] === tablero[b] && tablero[a] === tablero[c]) { //
      return tablero[a]
    }
  }
  return null
}
const ganador = computed(() => saberGanador(tablero.value.flat()))
const jugada = (x, y) => {
	if (ganador.value) return
	if (tablero.value[x][y]) return
	tablero.value[x][y] = jugador.value
	jugador.value = jugador.value === 'X' ? 'O' : 'X' //Operador ternario que cambia de turno al jugador que ya hizo su jugada.
}
const reiniciarElJuego = () => {
	tablero.value = [   /* Éste es el tablero pincipal, el que aparecerá sin datos cuando se visualice la aplicación web*/
		['', '', ''],
		['', '', ''],
		['', '', '']
	]
	jugador.value = 'X'  //Se le asigna la referencia X al usuario para detectar su jugada una vez que la haga.
}
</script>

<template>
	<main class="text-center">
		<h1>Taller 2</h1>
		<div class="flex flex-col items-center mb-8">
			<div 
				v-for="(row, x) in tablero" 
				:key="x"
				class="flex">
				<div 
					v-for="(cell, y) in row" 
					:key="y" 
					@click="jugada(x, y)" 
					:class="`border-2 border-blue  w-40 h-40 flex items-center justify-center material-icons-outlined text-5xl cursor-pointer ${cell === 'X' ? 'text-yellow-500' : 'text-red-900'}`">
					{{ cell === 'X' ? 'X' : cell === 'O' ? '0' : '' }} <!--Ternario que define las marcas de cada jugador, y deja vacío el tablero en caso de que no hayan jugadas-->
				</div>
			</div>
		</div>

		<div class="text-center">
      <button @click="reiniciarElJuego" class="px-6 py-4 bg-violet-500 rounded font-bold hover:bg-violet-800 duration-300">Reiniciar</button>
	
				</div>
	</main>
</template>

<style>
body {
	@apply bg-cyan-900 text-white;
}
</style>