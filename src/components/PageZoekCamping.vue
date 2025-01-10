<template>
    <div>
        <h1 class="font-bold text-[25px]">
            Camping zoeken
        </h1>
        <p>
            Op deze pagina kan je een camping zoeken.
        </p>

        <div class="space-y-4 bg-white p-6 rounded shadow-md w-full max-w-md mx-auto">
            <div class="flex items-center">
                <label for="searchLand" class="mr-2 w-1/3">Zoek op Land:</label>
                <input type="text" id="searchLand" v-model="filters.land" @input="fetchCampings" class="block w-2/3 border border-gray-300 rounded-md p-2">
            </div>
            <div class="flex items-center">
                <label for="searchGemeente" class="mr-2 w-1/3">Zoek op Gemeente:</label>
                <input type="text" id="searchGemeente" v-model="filters.gemeente" @input="fetchCampings" class="block w-2/3 border border-gray-300 rounded-md p-2">
            </div>
            <div class="flex items-center">
                <label for="filterAnimatie" class="mr-2 w-1/3">Animatie:</label>
                <input type="checkbox" id="filterAnimatie" v-model="filters.animatie" @change="fetchCampings" class="mt-1">
            </div>
            <div class="flex items-center">
                <label for="filterZwembad" class="mr-2 w-1/3">Zwembad:</label>
                <input type="checkbox" id="filterZwembad" v-model="filters.zwembad" @change="fetchCampings" class="mt-1">
            </div>
            <div class="flex items-center">
                <label for="filterSpeeltuin" class="mr-2 w-1/3">Speeltuin:</label>
                <input type="checkbox" id="filterSpeeltuin" v-model="filters.speeltuin" @change="fetchCampings" class="mt-1">
            </div>
        </div>

        <ul class="flex flex-wrap justify-center -mx-2 m-4">
            <li v-for="(camping, index) in campings" :key="index" class="border border-gray-300 rounded p-4 mb-4 w-full sm:w-1/2 md:w-1/3 lg:w-1/4 xl:w-1/5 mx-2">
                <p class="font-bold">{{ camping.Naam }}</p>
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