<template>
    <div>
        <h1>
            Gegevens camping aanpassen
        </h1>

        <div v-if="isIngelogd">

            <div>
                <label for="campingSelect">Selecteer een camping:</label>
                <select id="campingSelect" v-model="campingId" @change="fetchCampingDetails">
                    <option v-for="camping in campings" :key="camping.ID" :value="camping.ID">{{ camping.Naam }}</option>
                </select>
            </div>
            
            <form v-if="editCamping" @submit.prevent="updateCamping">
                <div>
                    <label for="naam">Naam:</label>
                    <input type="text" id="naam" v-model="editCamping.Naam" required>
                </div>
                <div>
                    <label for="plaatsElectriciteit">Plaats met Electriciteit:</label>
                    <input type="number" id="plaatsElectriciteit" v-model="editCamping.Plaats_Electriciteit" required>
                </div>
                <div>
                    <label for="plaatsZonderElectriciteit">Plaats zonder Electriciteit:</label>
                    <input type="number" id="plaatsZonderElectriciteit" v-model="editCamping.Plaats_Zonder_Electriciteit" required>
                </div>
                <div>
                    <label for="zwembad">Zwembad:</label>
                    <input type="checkbox" id="zwembad" v-model="editCamping.Zwembad">
                </div>
                <div>
                    <label for="speeltuin">Speeltuin:</label>
                    <input type="checkbox" id="speeltuin" v-model="editCamping.Speeltuin">
                </div>
                <div>
                    <label for="animatie">Animatie:</label>
                    <input type="checkbox" id="animatie" v-model="editCamping.Animatie">
                </div>
                <div>
                    <label for="straatnaam">Straatnaam:</label>
                    <input type="text" id="straatnaam" v-model="editCamping.Straatnaam" required>
                </div>
                <div>
                    <label for="huisnummer">Huisnummer:</label>
                    <input type="text" id="huisnummer" v-model="editCamping.Huisnummer" required>
                </div>
                <div>
                    <label for="postcode">Postcode:</label>
                    <input type="text" id="postcode" v-model="editCamping.Postcode" required>
                </div>
                <div>
                    <label for="gemeente">Gemeente:</label>
                    <input type="text" id="gemeente" v-model="editCamping.Gemeente" required>
                </div>
                <div>
                    <label for="land">Land:</label>
                    <input type="text" id="land" v-model="editCamping.Land" required>
                </div>
                <div>
                    <label for="bescrijving">Beschrijving:</label>
                    <textarea id="bescrijving" v-model="editCamping.Bescrijving" required></textarea>
                </div>
                <button type="submit">Update Camping</button>
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
        name: 'PageUpdateCamping',
        data() {
            return {
                userId: null,
                isIngelogd: false,
                campings: [],
                editCamping: [],
                campingId: null
            };
        },
        methods: {
            async updateCamping() {
                try {
                    console.log('updateCamping', this.campingId);
                    const response = await fetch("http://localhost:3000/api/camping/" + this.campingId, {
                    method: 'PUT',
                    headers: {
                        'Content-Type': 'application/json'  
                    },
                    body: JSON.stringify(this.editCamping),
                    credentials: 'include' // Include credentials (cookies)
                    });
                    if (response.ok) {
                        alert('Camping gegevens aangepast!');
                    } else {
                        console.log('Failed to update Camping!');
                        console.log(response);
                    }
                } catch (error) {
                    console.error(error);
                    }
            },
            async fetchCampings() {
                const response =  await fetch("http://localhost:3000/api/campingInBeheer/", {
                    credentials: 'include'
                });
                const data = await response.json();
                this.campings = data;
            },
            async fetchCampingDetails() {
                const response = await fetch("http://localhost:3000/api/camping/" + this.campingId, {
                    credentials: 'include'
                });
                const data = await response.json();
                console.log(data);
                console.log('campingId:', this.campingId);
                this.editCamping = data;
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
            this.fetchCampings();
        }
    }
</script>