<template>
    <div>
        <h1>
            Camping zoeken
        </h1>
        <p>
            Op deze pagina kan je een camping zoeken.
        </p>
        <p>Als je een plaats wilt boeken moet je een account aanmaken of inloggen.</p>
        <p>Als je een campingeignaar bent kan je een account aanmaken als campingbeheerder om jou camping toe te voegen.</p>


            <div>
                <label for="searchLand">Zoek op Land:</label>
                <input type="text" id="searchLand" v-model="filters.land" @input="fetchCampings">
            </div>
            <div>
                <label for="searchGemeente">Zoek op Gemeente:</label>
                <input type="text" id="searchGemeente" v-model="filters.gemeente" @input="fetchCampings">
            </div>
            <div>
                <label for="filterAnimatie">Animatie:</label>
                <input type="checkbox" id="filterAnimatie" v-model="filters.animatie" @change="fetchCampings">
            </div>
            <div>
                <label for="filterZwembad">Zwembad:</label>
                <input type="checkbox" id="filterZwembad" v-model="filters.zwembad" @change="fetchCampings">
            </div>
            <div>
                <label for="filterSpeeltuin">Speeltuin:</label>
                <input type="checkbox" id="filterSpeeltuin" v-model="filters.speeltuin" @change="fetchCampings">
            </div>

            <ul>
                <li v-for="(camping, index) in campings" :key="index">
                    <p>{{ camping.Naam }}</p>
                    <p>Plaatsen met electriciteit: {{ camping.Plaats_Electriciteit }}</p>
                    <p>Plaatsen zonder electriciteit: {{ camping.Plaats_Zonder_Electriciteit }}</p>
                    <p>Zwembad: {{ camping.Zwembad ? 'Ja' : 'Nee' }}</p>
                    <p>Speeltuin: {{ camping.Speeltuin ? 'Ja' : 'Nee' }}</p>
                    <p>Animatie: {{ camping.Animatie ? 'Ja' : 'Nee' }}</p>
                    <p>Adres: {{ camping.Straatnaam + " " + camping.Huisnummer + ", " + camping.Postcode + " " + camping.Gemeente + ", " + camping.Land }}</p>
                    <p>Beschrijving: {{ camping.Bescrijving }}</p>
                </li>
            </ul>
    </div>
</template>

<script>


    export default {
        name: 'PageZoekCamping',
        data() {
            return {
                userId: null,
                isIngelogd: false,
                campings: [],
                filters: {
                    animatie: false,
                    zwembad: false,
                    speeltuin: false,
                    land: '',
                    gemeente: ''
                }
            };
        },
        methods: {
            async fetchCampings() {
                const filters = {
                    ...this.filters,
                    animatie: this.filters.animatie ? 1 : 0,
                    zwembad: this.filters.zwembad ? 1 : 0,
                    speeltuin: this.filters.speeltuin ? 1 : 0
                }
                const response = await fetch(`http://localhost:3000/api/campings`, {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json'
                    },
                    body: JSON.stringify(filters),
                    credentials: 'include'
                });
                const data = await response.json();
                this.campings = data;
            }
        },
        created() {
            this.fetchCampings();
        }
    }
</script>