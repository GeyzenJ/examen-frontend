<template>
    <div>
        <h1>
            Camping toevoegen
        </h1>

        <div v-if="isIngelogd">

            <p>hoi je bent ingelog</p>
            <form @submit.prevent="addCamping">
                <div>
                    <label for="naam">Naam:</label>
                    <input type="text" id="naam" v-model="camping.Naam" required>
                </div>
                <div>
                    <label for="plaatsElectriciteit">Plaats met Electriciteit:</label>
                    <input type="number" id="plaatsElectriciteit" v-model="camping.Plaats_Electriciteit" required>
                </div>
                <div>
                    <label for="plaatsZonderElectriciteit">Plaats zonder Electriciteit:</label>
                    <input type="number" id="plaatsZonderElectriciteit" v-model="camping.Plaats_Zonder_Electriciteit" required>
                </div>
                <div>
                    <label for="zwembad">Zwembad:</label>
                    <input type="checkbox" id="zwembad" v-model="camping.Zwembad">
                </div>
                <div>
                    <label for="speeltuin">Speeltuin:</label>
                    <input type="checkbox" id="speeltuin" v-model="camping.Speeltuin">
                </div>
                <div>
                    <label for="animatie">Animatie:</label>
                    <input type="checkbox" id="animatie" v-model="camping.Animatie">
                </div>
                <div>
                    <label for="straatnaam">Straatnaam:</label>
                    <input type="text" id="straatnaam" v-model="camping.Straatnaam" required>
                </div>
                <div>
                    <label for="huisnummer">Huisnummer:</label>
                    <input type="text" id="huisnummer" v-model="camping.Huisnummer" required>
                </div>
                <div>
                    <label for="postcode">Postcode:</label>
                    <input type="text" id="postcode" v-model="camping.Postcode" required>
                </div>
                <div>
                    <label for="gemeente">Gemeente:</label>
                    <input type="text" id="gemeente" v-model="camping.Gemeente" required>
                </div>
                <div>
                    <label for="land">Land:</label>
                    <input type="text" id="land" v-model="camping.Land" required>
                </div>
                <div>
                    <label for="bescrijving">Beschrijving:</label>
                    <textarea id="bescrijving" v-model="camping.Bescrijving" required></textarea>
                </div>
                <button type="submit">Camping toevoegen</button>
            </form>
        
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
        name: 'PageAddCamping',
        data() {
            return {
                userId: null,
                isIngelogd: false,
                camping: {
                Naam: '',
                Plaats_Electriciteit: 0,
                Plaats_Zonder_Electriciteit: 0, 
                Zwembad: 0,
                Speeltuin: 0, 
                Animatie: 0,
                Straatnaam: '', 
                Huisnummer: '', 
                Postcode: '', 
                Gemeente: '', 
                Land: '', 
                Bescrijving: ''
                }
            };
        },
        methods: {
            addCamping() {
               fetch('http://localhost:3000/api/camping', {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json'
                    },
                    body: JSON.stringify({...this.camping, User_ID: this.userId}),
                    credentials: 'include'
                })
                .then(response => response.json())
                .then(data => {
                    if (data) {
                        alert('Camping toegevoegd!');
                    } else {
                        alert('Het is niet gelukt om een camping toe te voegen.');
                    }
            })},
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
        }
    }
</script>