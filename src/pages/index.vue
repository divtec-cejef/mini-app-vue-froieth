<template>
  <h1>Viepion</h1>

  <p>Joueur X : {{ pointsJoueurX }}</p>
  <p>Joueur Y : {{ pointsJoueurY }}</p>

  <p v-if="gagnant">Le joueur {{ gagnant }} a gagné !</p>
  <p v-else>C'est au tour du joueur {{ joueurActuel }}</p>

  <table>
    <tr v-for="(row, rowIndex) in 3" :key="rowIndex">
      <td v-for="(col, colIndex) in 3" :key="colIndex">
        <button
          :disabled="gagnant || boutons[rowIndex * 3 + colIndex] !== ''"
          @click="changerTextBouton(rowIndex * 3 + colIndex)"
        >
          {{ boutons[rowIndex * 3 + colIndex] }}
        </button>
      </td>
    </tr>
  </table>

  <button @click="resetJeu">Manche suivante</button>
</template>

<script setup>
  import { ref, watch } from 'vue'

  // État du jeu
  const boutons = ref(Array.from({ length: 9 }, () => ''))
  const premierJoueur = ref('X')
  const joueurActuel = ref(premierJoueur.value)
  const gagnant = ref(null)
  const nombreCoups = ref(1)

  // Points réactifs et persistants
  const pointsJoueurX = ref(parseInt(localStorage.getItem('pointsX')) || 0)
  const pointsJoueurY = ref(parseInt(localStorage.getItem('pointsY')) || 0)

  // Synchronisation avec localStorage
  watch(pointsJoueurX, val => {
    localStorage.setItem('pointsX', val)
  })
  watch(pointsJoueurY, val => {
    localStorage.setItem('pointsY', val)
  })

  // Fonction pour jouer
  function changerTextBouton (index) {
    if (gagnant.value || boutons.value[index] !== '') return
    boutons.value[index] = joueurActuel.value
    verifierGagnant()
    if (!gagnant.value) {
      joueurActuel.value = joueurActuel.value === 'X' ? 'O' : 'X'
      nombreCoups.value++
    }
  }

  // Combinaisons gagnantes
  const combinaisonsGagnantes = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ]

  // Vérification du gagnant
  function verifierGagnant () {
    for (const combo of combinaisonsGagnantes) {
      const [a, b, c] = combo
      const val = boutons.value[a]
      if (val && val === boutons.value[b] && val === boutons.value[c]) {
        if (val === 'X') {
          pointsJoueurX.value++
        } else {
          pointsJoueurY.value++
        }
        gagnant.value = val
      }
    }
  }

  // Réinitialiser le jeu
  function resetJeu () {
    if (gagnant.value !== null || nombreCoups.value === 10) {
      boutons.value = Array.from({ length: 9 }, () => '')
      gagnant.value = null
      premierJoueur.value = premierJoueur.value === 'X' ? 'O' : 'X'
      joueurActuel.value = premierJoueur.value
      nombreCoups.value = 1
    }
  }
</script>


<style scoped>
table {
  border-collapse: collapse;
  margin-bottom: 1rem;
}
td {
  border: 1px solid white;
  padding: 10px;
}
button {
  width: 60px;
  height: 40px;
  font-size: 1.2rem;
  cursor: pointer;
}
button:disabled {
  cursor: not-allowed;
}
</style>
