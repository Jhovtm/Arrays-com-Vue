
<script setup lang="ts">

import { ref } from 'vue'

const nomes = ref<string[]>([])

function mostrarInventores() {
  nomes.value = nome_completo(inventores)
}

const titulo = 'Lista de inventores'

interface PessoasImportantes {
    first: string
    last: string
    year?: number
    passed?: number
}

const inventores: PessoasImportantes[] = [
    { first: 'Albert', last: 'Einstein', year: 1879, passed: 1955 },
    { first: 'Isaac', last: 'Newton', year: 1643, passed: 1727 },
    { first: 'Galileo', last: 'Galilei', year: 1564, passed: 1642 },
    { first: 'Marie', last: 'Curie', year: 1867, passed: 1934 },
    { first: 'Johannes', last: 'Kepler', year: 1571, passed: 1630 },
    { first: 'Nicolaus', last: 'Copernicus', year: 1473, passed: 1543 },
    { first: 'Max', last: 'Planck', year: 1858, passed: 1947 },
    { first: 'Katherine', last: 'Blodgett', year: 1898, passed: 1979 },
    { first: 'Ada', last: 'Lovelace', year: 1815, passed: 1852 },
    { first: 'Sarah E.', last: 'Goode', year: 1855, passed: 1905 },
    { first: 'Lise', last: 'Meitner', year: 1878, passed: 1968 },
    { first: 'Hanna', last: 'Hammarström', year: 1829, passed: 1909 }
]

const people: string[] = [
    'Bernhard, Sandra', 'Bethea, Erin', 'Becker, Carl',
    'Bentsen, Lloyd', 'Beckett, Samuel', 'Blake, William',
    'Berger, Ric', 'Beddoes, Mick', 'Beethoven, Ludwig',
    'Belloc, Hilaire', 'Begin, Menachem', 'Bellow, Saul',
    'Benchley, Robert', 'Blair, Robert', 'Benenson, Peter',
    'Benjamin, Walter', 'Berlin, Irving', 'Benn, Tony',
    'Benson, Leana', 'Bent, Silas', 'Berle, Milton',
    'Berry, Halle', 'Biko, Steve', 'Beck, Glenn',
    'Bergman, Ingmar', 'Black, Elk', 'Berio, Luciano',
    'Berne, Eric', 'Berra, Yogi', 'Berry, Wendell',
    'Bevan, Aneurin', 'Ben-Gurion, David', 'Bevel, Ken',
    'Biden, Joseph', 'Bennington, Chester', 'Bierce, Ambrose',
    'Billings, Josh', 'Birrell, Augustine', 'Blair, Tony',
    'Beecher, Henry', 'Biondo, Frank'
]


function ano_de_nascimento(p: PessoasImportantes[]): PessoasImportantes[] {
    return p.filter(
        inventor =>
            inventor.year !== undefined &&
            inventor.year >= 1500 &&
            inventor.year <= 1599
    )
}

function tempoVivido (p: PessoasImportantes): any{

       if(p?.passed != undefined && p?.year != undefined){
        return p.passed-p.year;
       }else{
        return undefined
       }
}


function nome_completo(p: PessoasImportantes[]): string[] {
  return p.map(inventor => `${inventor.first} ${inventor.last}`)
}

function IdadeNome(p: PessoasImportantes[]): string[] {
  return p.map(inventor => `${inventor.first} ${inventor.last} viveu ${tempoVivido(inventor)} anos`)
}

function organiza_Nascimento(p: PessoasImportantes[]): PessoasImportantes[] {
    const pessoas = [...p]

    for (let i = 0; i < pessoas.length - 1; i++) {
        for (let j = 0; j < pessoas.length - 1 - i; j++) {
            const atual = pessoas[j]
            const proximo = pessoas[j + 1]

            if (
                atual &&
                proximo &&
                atual.year !== undefined &&
                proximo.year !== undefined &&
                atual.year > proximo.year
            ) {
                pessoas[j] = proximo
                pessoas[j + 1] = atual
            }
        }
    }

    return pessoas
}

function organizaIdade(p: PessoasImportantes[]): PessoasImportantes[] {
    const people = [...p]

    for (let i = 0; i < people.length - 1; i++) {
        for (let j = 0; j < people.length - 1 - i; j++) {
            const atual = people[j]
            const proximo = people[j + 1]

            if (
                atual &&
                proximo &&
                atual.year !== undefined &&
                atual.passed !== undefined &&
                proximo.year !== undefined &&
                proximo.passed !== undefined
            ) {
                const idadeAtual = atual.passed - atual.year
                const idadeProxima = proximo.passed - proximo.year

                if (idadeAtual > idadeProxima) {
                    people[j] = proximo
                    people[j + 1] = atual
                }
            }
        }
    }

    return people
}


function anos_juntos(p: PessoasImportantes[]): number {
    const validos = p.filter(
        pessoa => pessoa.year !== undefined &&
                  pessoa.passed !== undefined
    )

    if (validos.length === 0) return 0

    const ultimo_nascimento = Math.max(
        ...validos.map(pessoa => pessoa.year!)
    )

    const primeira_morte = Math.min(
        ...validos.map(pessoa => pessoa.passed!)
    )

    return Math.max(0, primeira_morte - ultimo_nascimento)
}

  const ultimoNascimento = Math.max(...inventores.map(pessoa => pessoa.year ?? 0))
  const primeiraMorte = Math.min(...inventores.map(pessoa => pessoa.passed ?? Infinity))
  const anosJuntos = anos_juntos(inventores)

const data = [
  'car', 'car', 'truck', 'truck', 'bike', 'walk',
  'car', 'van', 'bike', 'walk', 'car', 'van',
  'car', 'truck'
]

function removeRedundancias(p: string[]): string[] {
  return [...new Set(p)]
} 

const novoArray = removeRedundancias(data)
        

</script>

<template>
    <nav class="navbar is-dark" role="navigation">
  <div class="navbar-brand">
    <a class="navbar-item">
      <h1 class="title">Laboratório de Arrays</h1>
    </a>
  </div>
</nav>

<br>

<div class="botoes-organiza">
    <details>
  <summary class="button is-primary">
    Mostrar inventores
  </summary>

  <ul>
    <li v-for="nome in nome_completo(inventores)" :key="nome">
      {{ nome }}
    </li>
  </ul>
</details>

<details>
  <summary class="button is-primary">
    Inventores que nasceram entre 1500 e 1599
  </summary>

  <ul>
    <li v-for="nome in nome_completo(ano_de_nascimento(inventores))" :key="nome">
      {{ nome }}
    </li>
  </ul>
</details>

<details>
  <summary class="button is-primary">
    Anos em que os inventores estiveram vivos juntos
  </summary>

  <p>
    O último nascimento foi em: {{ ultimoNascimento }}
  </p>

  <p>
    A primeira morte foi em: {{ primeiraMorte }}
  </p>

  <p class="has-text-primary">
    Anos em comum: {{ anosJuntos }}
  </p>
</details>

<details>
  <summary class="button is-primary">
    Ordenar inventores pelos anos vividos
  </summary>

  <ul>
    <li v-for="nome in IdadeNome(organizaIdade(inventores))" :key="nome">
  {{ nome }}
</li>
  </ul>
</details>


</div>

<div class="botoes-organiza">
<details>
  <summary class="button is-primary">
    Remover redundâncias
  </summary>

  <ul>
    <li v-for="item in removeRedundancias(data)" :key="item">
      {{ item }}
    </li>
  </ul>
</details>
</div>



</template>




<style>

    .botoes-organiza{
        display: flex;
        gap: 1rem;
        margin: 0.3rem;
    }

    li{
        color: aliceblue;
        margin: 1rem;
    }


</style>