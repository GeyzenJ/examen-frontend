<template>
    <div>
        <h1 class="font-bold text-[25px]">
            Overzicht boekingen
        </h1>

        <div v-if="isIngelogd">
            <div v-if="overzichtBoekingen.length == 0">
                <p>
                    Geen boekingen!
                </p>
            </div>
        <div v-if="overzichtBoekingen != 0">
                <h2 class="font-bold text-[15px]">Aanstaande boekingen</h2>
                <ul class="flex flex-wrap justify-center m-4 -mx-2">
                    <li v-for="(boekeningen, index) in aanstaandeBoekingen" :key="index" class="border border-gray-300 rounded p-4 mb-4 w-full sm:w-1/2 md:w-1/3 lg:w-1/4 xl:w-1/5 mx-2">
                        <ul class="space-y-2">
                            <li>
                                <p>Camping: {{ boekeningen.Camping_Name }}</p>
                            </li>
                            <li>
                                <p>Omschrijving: {{ boekeningen.Bescrijving }}</p>
                            </li>
                            <li>
                                <p>Start datum: {{ new Date(boekeningen.Start_Datum).toLocaleDateString().split('T')[0] }}</p>
                                <p>Eind datum: {{ new Date(boekeningen.Eind_Datum).toLocaleDateString().split('T')[0] }}</p>
                            </li>
                            <li>
                                <p v-if="boekeningen.Electriciteit == 1">Plaats met electriciteit: Ja</p>
                                <p v-if="boekeningen.Electriciteit == 0">Plaats met electriciteit: Nee</p>
                            </li>
                        </ul>
                    </li>
                </ul>
                <div v-if="gepasseerdeBoekingen.length != 0">
                    <h2 class="font-bold text-[15px]">Gepasseeerde boekingen</h2>
                    <ul class="flex flex-wrap justify-center m-4 -mx-2">
                        <li v-for="(boekeningen, index) in gepasseerdeBoekingen" :key="index" class="border border-gray-300 rounded p-4 mb-4 w-full sm:w-1/2 md:w-1/3 lg:w-1/4 xl:w-1/5 mx-2">
                            <ul class="space-y-2">
                                <li>
                                    <p>Camping: {{ boekeningen.Camping_Name }}</p>
                                </li>
                                <li>
                                    <p>Omschrijving: {{ boekeningen.Bescrijving }}</p>
                                </li>
                                <li>
                                    <p>Start datum: {{ new Date(boekeningen.Start_Datum).toLocaleDateString().split('T')[0] }}</p>
                                    <p>Eind datum: {{ new Date(boekeningen.Eind_Datum).toLocaleDateString().split('T')[0] }}</p>
                                </li>
                                <li>
                                    <p v-if="boekeningen.Electriciteit == 1">Plaats met electriciteit: Ja</p>
                                    <p v-if="boekeningen.Electriciteit == 0">Plaats met electriciteit: Nee</p>
                                </li>
                            </ul>
                        </li>
                    </ul>

                </div>
            </div>
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
        name: 'PageOverzichtBoekingen',
        data() {
            return {
                userId: null,
                isIngelogd: false,
                overzichtBoekingen: [],   
                aanstaandeBoekingen: [],
                gepasseerdeBoekingen: []      
            };
        },
        methods: {
            async fetchBoekingen() {
                const response =  await fetch("http://localhost:3000/api/boekingenUser/" + this.userId);
                const data = await response.json();
                this.overzichtBoekingen = data;
                this.sorteerBoekingen();
            },
            sorteerBoekingen() {
                const vandaag = new Date().toISOString().split('T')[0];
                this.aanstaandeBoekingen = this.overzichtBoekingen.filter(boeking => boeking.Eind_Datum >= vandaag);
                this.gepasseerdeBoekingen = this.overzichtBoekingen.filter(boeking => boeking.Eind_Datum < vandaag);
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
                this.fetchBoekingen();
            }
        }
    }
</script>