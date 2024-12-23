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
            <ul v-for="(boekeningen, index) in overzichtBoekingen" :key="index">
                <li>
                    <ul>
                        <li>
                            <p>Camping: {{ boekeningen.Camping_Name }}</p>
                        </li>
                        <li>
                          <p>Omschrijving:  {{  boekeningen.Bescrijving }}</p> 
                        </li>
                        <li>
                
                            <p>Start datum: {{ new Date(boekeningen.Start_Datum).toISOString().split('T')[0] }} </p>
                            <p>Eind datum: {{new Date(boekeningen.Eind_Datum).toISOString().split('T')[0] }} </p>
                        </li>
                    </ul>
                   
                     
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
        name: 'PageOverzichtBoekingen',
        data() {
            return {
                userId: null,
                isIngelogd: false,
                overzichtBoekingen: [],         
            };
        },
        methods: {
            async fetchBoekingen() {
                const response =  await fetch("http://localhost:3000/api/boekingenUser/" + this.userId);
                const data = await response.json();
                this.overzichtBoekingen = data;
                console.log('fetch boekingen');
                console.log(data);
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