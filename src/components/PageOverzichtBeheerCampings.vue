<template>
    <div>
        <h1 class="font-bold text-[25px]">
            Overzicht campings in beheer
        </h1>

        <div v-if="isIngelogd">

            <ul class="flex flex-wrap justify-center m-4 -mx-2">
                <li v-for="(camping, index) in campings" :key="index" class="border border-gray-300 rounded p-4 mb-4 w-full sm:w-1/2 md:w-1/3 lg:w-1/4 xl:w-1/5 mx-2">
                    <p class="font-bold">{{ camping.Naam }}</p>
                    <p>Plaatsen met electriciteit: {{ camping.Plaats_Electriciteit }}</p>
                    <p>Plaatsen zonder electriciteit: {{ camping.Plaats_Zonder_Electriciteit }}</p>
                    <p v-if="camping.Zwembad == 1">Zwembad: ja</p>
                    <p v-if="camping.Zwembad == 0">Zwembad: nee</p>
                    <p v-if="camping.Speeltuin == 1">Speeltuin: ja</p>
                    <p v-if="camping.Speeltuin == 0">Speeltuin: nee</p>
                    <p v-if="camping.Animatie == 1">Kinderanimatie: ja</p>
                    <p v-if="camping.Animatie == 0">Kinderanimatie: nee</p>
                    <p>Adres: {{ camping.Straatnaam + " " + camping.Huisnummer + ", " + camping.Postcode + " " + camping.Gemeente + ", " + camping.Land }}</p>
                    <p>Beschrijving: {{ camping.Bescrijving }}</p>
                </li>
            </ul>
        </div>
        <div v-if="!isIngelogd">
            <p>
                Niet ingelogd!
            </p>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'PageOverzichtBeheerCampings',
        data() {
            return {
                userId: null,
                isIngelogd: false,
                campings: []  
            };
        },
        methods: {
            async fetchCampingsInBeheer() {
                const respons = await fetch("http://localhost:3000/api/campingInBeheer/", {
                    credentials: 'include'
                });
                const data = await respons.json();
                this.campings = data;
            },
            checkLoginStatus() {
                this.userId = this.$cookies.get('userId');
                if (this.userId)
                {
                    this.isIngelogd = true;
                }
            }
        },
        created() {
            this.checkLoginStatus();
            if (this.isIngelogd) {
                this.fetchCampingsInBeheer();
            }
        }
    }
</script>