<template>
    <div>
        <h1>
            Overzicht Campings
        </h1>

        <div v-if="isIngelogd">

        <ul>
            <li v-for="(camping, index) in campings" :key="index">
                <p>{{ camping.Naam }}</p>
                <p> Plaatsen met elctriciteit: {{ camping.Plaats_Electriciteit }}</p>
                <p>Plaatsen zonder electricitet: {{ camping.Plaats_Zonder_Electriciteit }}</p>
                <p v-if="camping.Zwembad == 1"> Zwembad: ja</p>
                <p v-if="camping.Zwembad == 0"> Zwembad: nee</p>
                <p v-if="camping.Speeltuin == 1"> Speeltuin: ja</p>
                <p v-if="camping.Speeltuin == 0"> Speeltuin: nee</p>
                <p v-if="camping.Animatie == 1"> Kinderanimatie: ja</p>
                <p v-if="camping.Animatie == 0"> Kinderanimatie: nee</p>
                <p>Adres: {{ camping.Straatnaam + " " + camping.Huisnummer + ", " + camping.Postcode + " "+ camping.Gemeente + ", " + camping.Land }}</p>
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