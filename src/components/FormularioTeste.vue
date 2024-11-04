<template>
    <form @submit.prevent="submitForm">
        <div>
            <!-- Step 1: Vacinação -->
            <div v-if="step === 1">
                <h3>Você se vacinou nos últimos 30 dias?</h3>
                <div class="radio-group">
                    <label :class="{ 'checked': vacinacao === 'sim' }">
                        <input type="radio" value="sim" v-model="vacinacao">
                        Sim
                    </label>
                    <label :class="{ 'checked': vacinacao === 'nao' }">
                        <input type="radio" value="nao" v-model="vacinacao">
                        Não
                    </label>
                </div>
                <div v-if="vacinacao === 'sim'">
                    <h3>Selecione as vacinas que você tomou:</h3>
                    <div v-for="(vacina, index) in vacinas" :key="index">
                        <select v-model="vacina.selected">
                            <option v-for="option in listaVacinas" :key="option.value" :value="option.value">
                                {{ option.text }}
                            </option>
                        </select>
                        <input type="date" v-model="vacina.diaVacina" @change="daysFromVaccination(index)">
                        <div v-if="vacina.selected === 'covid'">
                            <h3>A vacina que você tomou foi Coronavac ou Covaxin?</h3>
                            <div class="radio-group-small">
                                <label :class="{ 'checked': vacina.coronavac === 'sim' }">
                                    <input type="radio" value="sim" v-model="vacina.coronavac">
                                    Sim
                                </label>
                                <label :class="{ 'checked': vacina.coronavac === 'nao' }">
                                    <input type="radio" value="nao" v-model="vacina.coronavac">
                                    Não
                                </label>
                            </div>
                        </div>
                        <button v-if="index === vacinas.length - 1" @click="addVaccine">Adicionar outra Vacina</button>
                    </div>
                </div>
            </div>
        </div>
    </form>

</template>

<script>
export default {
    name: 'FormularioTeste',
    data() {
        return {
            step: 1,
            vacinacao: '',
            diaVacina: '',
            diasDesdeVacinacao: '',
            listaVacinas: [
                { value: 'antirrabica_profilatica', text: 'Antirrábica profilática' },
                { value: 'antirrabica_apos_animal', text: 'Antirrábica após exposição animal' },
                { value: 'bcg', text: 'BCG' },
                { value: 'brucelose', text: 'Brucelose' },
                { value: 'caxumba', text: 'Caxumba (Parotidite)' },
                { value: 'colera', text: 'Cólera' },
                { value: 'coqueluche', text: 'Coqueluche' },
                { value: 'covid', text: 'Covid-19' },
                { value: 'dengue', text: 'Dengue' },
                { value: 'difteria', text: 'Difteria' },
                { value: 'febre_amarela', text: 'Febre amarela' },
                { value: 'febre_tifoide_oral', text: 'Febre Tifoide Oral' },
                { value: 'febre_tifoife_injetavel', text: 'Febre Tifoide (Injetável)' },
                { value: 'hepatite_a', text: 'Hepatite A' },
                { value: 'hepatite_b_recombinante', text: 'Hepatite B recombinante' },
                { value: 'hepatite_b_plasma', text: 'Hepatite B (derivada de plasma)' },
                { value: 'hpv', text: 'HPV' },
                { value: 'hemophillus', text: 'Hemophillus influenzae' },
                { value: 'influenza', text: 'Influenza (gripe)' },
                { value: 'leptospirose', text: 'Leptospirose' },
                { value: 'meningite', text: 'Meningite' },
                { value: 'peste', text: 'Peste' },
                { value: 'pneumococo', text: 'Pneumococo' },
                { value: 'polio_oral', text: 'Pólio Oral (Sabin)' },
                { value: 'polio', text: 'Pólio (Salk)' },
                { value: 'rubeola', text: 'Rubéola' },
                { value: 'soro_antitetanico', text: 'Soro Antitetânico' },
                { value: 'sarampo', text: 'Sarampo' },
                { value: 'tetano', text: 'Tétano' },
                { value: 'varicela', text: 'Varicela (Catapora)' },
                { value: 'variola', text: 'Varíola' },
            ],
            vacinas: [{
                selected: '',
                diaVacina: '',
                coronavac: ''
            }],
        };
    },
    methods: {
        addVaccine() {
            this.vacinas.push({ selected: null, diaVacina: '', coronavac: null })
        },
        daysFromVaccination(index) {
            if (this.vacinas[index].diaVacina) {
                const dataVacina = new Date(this.vacinas[index].diaVacina);
                const dataAtual = new Date();
                const diferencaEmMilissegundos = dataAtual - dataVacina;
                const dias = Math.floor(diferencaEmMilissegundos / (1000 * 60 * 60 * 24));
                this.vacinas[index].diasDesdeVacinacao = dias;
            } else {
                this.vacinas[index].diasDesdeVacinacao = null;
            }
        },
    },
};
</script>


<style scoped>
/* Estilos da barra de progresso */

.progress-bar-container {
    position: relative;
    width: 100%;
    height: 5px;
    background-color: #F0F0F0;
    border-radius: 2.5px;
    margin-bottom: 40px;
}

.progress-bar {
    height: 100%;
    background-color: #E90B0B;
    border-radius: 5px;
    transition: 0.3s ease-in-out;
}

.progress-steps {
    position: absolute;
    top: -5px;
    width: 100%;
    display: flex;
    justify-content: space-between;
}

.progress-step {
    width: 15px;
    height: 15px;
    background-color: #F0F0F0;
    border-radius: 50%;
    transition: background-color 0.3s ease-in-out;
}

.progress-step.active {
    background-color: #E90B0B;
}

/* Estilos dos textos */
h2 {
    color: #000000;
    font-size: 1.4em;
    margin: 10px 0 30px;
    font-weight: normal;
    text-align: center;
}

h3 {
    color: #000000;
    font-size: 1.1em;
    margin-bottom: 10px;
    font-weight: normal;
}

/* Estilo dos inputs */

.input-container {
    position: relative;
    width: 100%;
}

.input-container::after {
    position: absolute;
    right: 35px;
    top: 50%;
    transform: translateY(-50%);
    color: #555;
}

#input-peso::after {
    content: 'kg';
}

#input-altura::after {
    content: 'm';
}

#input-idade::after {
    content: 'anos';
}

input[type="number"],
select {
    display: block;
    width: 100%;
    box-sizing: border-box;
    border: 1px solid #BBB;
    border-radius: 5px;
    color: #555;
    padding: 6px 10px;
    margin-bottom: 25px;
}

input[type="number"]:active select:active {
    border: 1px solid #BBB;
}

.vue-select {
    margin-bottom: 25px;
}

/* Estilo dos radio buttons */
.radio-group,
.radio-group-small {
    display: flex;
    margin-bottom: 25px;
}

.radio-group label,
.radio-group-small label {
    cursor: pointer;
    text-align: center;
    border: 1px solid #BBB;
    transition: background-color 0.3s ease, border-color 0.3s ease;
}

.radio-group label {
    width: 50%;
    padding: 10px 20px;
}

.radio-group-small label {
    width: 15%;
    padding: 5px 10px;
}


.radio-group label:first-child,
.radio-group-small label:first-child {
    border-top-left-radius: 5px;
    border-bottom-left-radius: 5px;
}

.radio-group label:last-child,
.radio-group-small label:last-child {
    border-top-right-radius: 5px;
    border-bottom-right-radius: 5px;
}

.radio-group input[type="radio"],
.radio-group-small input[type="radio"] {
    display: none;
    margin: 0 10px;
}

.radio-group label.checked,
.radio-group-small label.checked {
    background-color: #E90B0B;
    color: #FFF;
    border-color: #E90B0B;
}

/* Estilo das mensagens de erro */
.error-message {
    color: red;
    font-size: 0.8em;
    margin-top: -15px;
    margin-bottom: 15px;
}


/* Estilos dos botões */

.navigation-buttons {
    display: flex;
    flex-direction: row;
    align-items: end;
    justify-content: space-between;
    margin-top: 15px;
}

.button_prev,
.button_next,
.button_finish {
    border-radius: 5px;
    padding: 8px 10px;
}

.button_prev {
    background-color: #DDD;
    color: #111;
}

.button_next {
    background-color: #E90B0B;
    color: #FFF;
    margin-left: auto;
}

.button_finish {
    background-color: #0BB54C;
    color: #FFFFFF;
}
</style>