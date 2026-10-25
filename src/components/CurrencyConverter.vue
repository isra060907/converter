<script setup>
import { ref } from 'vue'
import { devises, taux } from '../data/taux.js'

const montant = ref('0')
const deviseSource = ref('USD')
const deviseCible = ref('EUR')
const resultat = ref('')
const erreur = ref(false)

function appuyer(touche) {
  if (touche === 'C') {
    montant.value = '0'
    resultat.value = ''
    erreur.value = false
  }
  else if (touche === '.') {
    if (!montant.value.includes('.') && montant.value.length < 10) {
      montant.value = montant.value + touche
      erreur.value = false
    }
  }
  else {
    if (montant.value === '0') {
      montant.value = touche
    }
    else if (montant.value.length < 10) {
      montant.value = montant.value + touche
    }

    erreur.value = false
  }
}
function convertir() {
  if (montant.value === '0') {
    erreur.value = true
    resultat.value = ''
    return
  }
  erreur.value = false
  const montantEUR = parseFloat(montant.value) / taux[deviseSource.value]
  const valeur = montantEUR * taux[deviseCible.value]
  resultat.value = valeur.toFixed(2)
}
function inverser() {
  const ancienneSource = deviseSource.value
  deviseSource.value = deviseCible.value
  deviseCible.value = ancienneSource

  if (resultat.value !== '') {
    convertir()
  }
}
</script>

<template>
  <div>
    <h1>Currency Converter</h1>

    <select v-model="deviseSource">
      <option
        v-for="d in devises"
        :key="d"
        :value="d"
      >
        {{ d }}
      </option>
    </select>

    <div>
      {{ montant }}
    </div>

    <button class="bouton-inverser" @click="inverser">⇅</button>

    <select v-model="deviseCible">
      <option
        v-for="d in devises"
        :key="d"
        :value="d"
      >
        {{ d }}
      </option>
    </select>

    <div>
      {{ resultat }}
    </div>

    <button class="bouton-convertir" @click="convertir">Convertir</button>
    <p v-if="erreur">Veuillez saisir un montant</p>
    <div class="clavier">
      <button
        v-for="touche in ['7', '8', '9', '4', '5', '6', '1', '2', '3', 'C', '0', '.']"
        :key="touche"
        @click="appuyer(touche)"
        :class="{ 'touche-effacer': touche === 'C' }"
      >
        {{ touche }}
      </button>
    </div>
  </div>
</template>
<style scoped>

.converter {
  width: 360px;
  max-width: 100%;
  margin: 20px auto;
  background-color: white;
  border-radius: 15px;
  overflow: hidden;
  box-shadow: 0 5px 20px rgba(0, 0, 0, 0.2);
}

.ecran {
  background-color: #2BB8E8;
  padding: 20px;
  color: white;
  text-align: center;
}

h1 {
  margin-bottom: 20px;
}

select {
  width: 100%;
  padding: 10px;
  margin: 5px 0;
  font-size: 18px;
}

.montant,
.resultat {
  font-size: 48px;
  text-align: right;
  border-bottom: 2px solid white;
  margin: 15px 0;
  min-height: 55px;
}

button {
  border: none;
  background: none;
  cursor: pointer;
}

.bouton-convertir {
  background-color: white;
  color: #1BA6C9;
  padding: 10px 25px;
  border-radius: 20px;
  font-size: 16px;
}

.clavier {
  padding: 20px;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 10px;
}

.clavier button {
  font-size: 32px;
  color: #1BA6C9;
  height: 60px;
}

.clavier .touche-effacer {
  color: #F0727A;
}

p {
  color: #F0727A;
  font-weight: bold;
}

* {
  box-sizing: border-box;
}

.converter {
  width: 360px;
  max-width: 100%;
  margin: 20px auto;
  background: white;
  border-radius: 15px;
  overflow: hidden;
  box-shadow: 0 5px 20px rgba(0, 0, 0, 0.15);
  font-family: Arial, sans-serif;
}

.ecran {
  background-color: #DFECFA;
  padding: 20px;
  color: #222;
}

h1 {
  text-align: center;
  font-size: 20px;
  margin: 0 0 25px;
}

select {
  width: 100px;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 6px;
  background: white;
  font-size: 14px;
}

.montant,
.resultat {
  font-size: 32px;
  text-align: right;
  border-bottom: 2px solid #aaa;
  margin: 10px 0 15px auto;
  min-height: 45px;
}

.bouton-inverser {
  display: block;
  font-size: 25px;
  color: #222;
  margin: 0 0 5px;
}

.bouton-convertir {
  display: block;
  margin: 20px auto 0;
  background-color: #8CB8EF;
  color: #222;
  padding: 10px 28px;
  border-radius: 25px;
  font-size: 14px;
  font-weight: bold;
}

.clavier {
  padding: 15px 25px;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 8px;
}

.clavier button {
  font-size: 26px;
  color: #5895ED;
  height: 42px;
}

.clavier .touche-effacer {
  color: #F0727A;
}

p {
  color: #F0727A;
  font-weight: bold;
  text-align: center;
}

</style>