<template>
    <div>
        <h1>
            Overzicht boekingen
        </h1>

        <div v-if="isIngelogd">
           <p>
            Je bent ingelogd!
           </p>
            <ul v-for="(boekeningen, index) in overzichtBoekingen" :key="index">
                <li>
                    {{ boekeningen.Camping_Name }} {{  boekeningen.Bescrijving }}
                    {{ boekeningen.Start_Datum }} {{boekeningen.Eind_Datum}}
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
                console.log('fetch aangeroepen')
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