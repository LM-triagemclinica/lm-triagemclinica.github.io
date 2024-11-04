<template>
    <main>
        <h1>Teste de triagem clínica</h1>
        <p>Esse teste consiste em uma série de perguntas para determinar se você está apto para doar sangue pelos
            critérios da triagem clínica.</p>
        <button @click="$emit('iniciar')">Iniciar</button>
    </main>
</template>

<script>
export default {
    name: 'PaginaInicial',
}
</script>

<style scoped>
main {
    align-items: center;
}

h1 {
    color: #C80505;
    font-family: Arial, Helvetica, sans-serif;
    font-weight: 400;
    font-size: 1.6em;
    text-align: center;
    margin-bottom: 40px;
}

button {
    background-color: #E90B0B;
    color: #FFF;
    font-size: 1.1em;
    width: 150px;
    padding: 12px;
    border-radius: 10px;
    margin-top: 60px;
}
</style>
