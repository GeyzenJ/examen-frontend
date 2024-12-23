<template>
    <div>
        <h1>
            Overzicht boekingen
        </h1>

        <div v-if="isIngelogd">
            <div v-if="overzichtBoekingen.length == 0">
                <p>
                    Geen boekingen!
                </p>
            </div>
        <div v-if="overzichtBoekingen != 0">
                <h2>Aanstaande boekingen</h2>
                <ul v-for="(boekeningen, index) in aanstaandeBoekingen" :key="index">
                    <li>
                        <ul>
                            <li>
                                <p>Camping: {{ boekeningen.Camping_Name }}</p>
                            </li>
                            <li>
                            <p>Omschrijving:  {{  boekeningen.Bescrijving }}</p> 
                            </li>
                            <li>
                                <p>Start datum: {{ new Date(boekeningen.Start_Datum).toLocaleDateString().split('T')[0] }} </p>
                                <p>Eind datum: {{new Date(boekeningen.Eind_Datum).toLocaleDateString().split('T')[0] }} </p>
                            </li>
                            <li>
                                <p v-if="boekeningen.Electriciteit == 1">
                                    Plaats met electricitet: Ja
                                </p>
                                <p v-if="boekeningen.Electriciteit == 0">
                                    Plaats met electricitet: Nee
                                </p>
                            </li>
                        </ul>
                    </li>
                </ul>
                <div v-if="gepasseerdeBoekingen.length != 0">
                    <h2 >Gepasseeerde boekingen</h2>
                    <ul v-for="(boekeningen, index) in gepasseerdeBoekingen" :key="index">
                        <li>
                            <ul>
                                <li>
                                    <p>Camping: {{ boekeningen.Camping_Name }}</p>
                                </li>
                                <li>
                                <p>Omschrijving:  {{  boekeningen.Bescrijving }}</p> 
                                </li>
                                <li>
                                    <p>Start datum: {{ new Date(boekeningen.Start_Datum).toLocaleDateString().split('T')[0] }} </p>
                                    <p>Eind datum: {{new Date(boekeningen.Eind_Datum).toLocaleDateString().split('T')[0] }} </p>
                                </li>
                                <li>
                                    <p v-if="boekeningen.Electriciteit == 1">
                                        Plaats met electricitet: Ja
                                    </p>
                                    <p v-if="boekeningen.Electriciteit == 0">
                                        Plaats met electricitet: Nee
                                    </p>
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