<template>
    <div>
        <h1 class="font-bold text-[25px]">
            Gegevens camping aanpassen
        </h1>

        <div v-if="isIngelogd">

            <div class="space-y-4 bg-white p-6 rounded shadow-md w-full max-w-md mx-auto">
                <div class="flex items-center">
                    <label for="campingSelect" class="mr-2 w-1/3">Selecteer een camping:</label>
                    <select id="campingSelect" v-model="campingId" @change="fetchCampingDetails" class="block w-2/3 border border-gray-300 rounded-md p-2">
                        <option v-for="camping in campings" :key="camping.ID" :value="camping.ID">{{ camping.Naam }}</option>
                    </select>
                </div>

                <form v-if="editCamping" @submit.prevent="updateCamping" class="space-y-4">
                    <div class="flex items-center">
                        <label for="naam" class="mr-2 w-1/3">Naam:</label>
                        <input type="text" id="naam" v-model="editCamping.Naam" required class="block w-2/3 border border-gray-300 rounded-md p-2">
                    </div>
                    <div class="flex items-center">
                        <label for="plaatsElectriciteit" class="mr-2 w-1/3">Plaats met Electriciteit:</label>
                        <input type="number" id="plaatsElectriciteit" v-model="editCamping.Plaats_Electriciteit" required class="block w-2/3 border border-gray-300 rounded-md p-2">
                    </div>
                    <div class="flex items-center">
                        <label for="plaatsZonderElectriciteit" class="mr-2 w-1/3">Plaats zonder Electriciteit:</label>
                        <input type="number" id="plaatsZonderElectriciteit" v-model="editCamping.Plaats_Zonder_Electriciteit" required class="block w-2/3 border border-gray-300 rounded-md p-2">
                    </div>
                    <div class="flex items-center">
                        <label for="zwembad" class="mr-2 w-1/3">Zwembad:</label>
                        <input type="checkbox" id="zwembad" v-model="editCamping.Zwembad" class="mt-1">
                    </div>
                    <div class="flex items-center">
                        <label for="speeltuin" class="mr-2 w-1/3">Speeltuin:</label>
                        <input type="checkbox" id="speeltuin" v-model="editCamping.Speeltuin" class="mt-1">
                    </div>
                    <div class="flex items-center">
                        <label for="animatie" class="mr-2 w-1/3">Animatie:</label>
                        <input type="checkbox" id="animatie" v-model="editCamping.Animatie" class="mt-1">
                    </div>
                    <div class="flex items-center">
                        <label for="straatnaam" class="mr-2 w-1/3">Straatnaam:</label>
                        <input type="text" id="straatnaam" v-model="editCamping.Straatnaam" required class="block w-2/3 border border-gray-300 rounded-md p-2">
                    </div>
                    <div class="flex items-center">
                        <label for="huisnummer" class="mr-2 w-1/3">Huisnummer:</label>
                        <input type="text" id="huisnummer" v-model="editCamping.Huisnummer" required class="block w-2/3 border border-gray-300 rounded-md p-2">
                    </div>
                    <div class="flex items-center">
                        <label for="postcode" class="mr-2 w-1/3">Postcode:</label>
                        <input type="text" id="postcode" v-model="editCamping.Postcode" required class="block w-2/3 border border-gray-300 rounded-md p-2">
                    </div>
                    <div class="flex items-center">
                        <label for="gemeente" class="mr-2 w-1/3">Gemeente:</label>
                        <input type="text" id="gemeente" v-model="editCamping.Gemeente" required class="block w-2/3 border border-gray-300 rounded-md p-2">
                    </div>
                    <div class="flex items-center">
                        <label for="land" class="mr-2 w-1/3">Land:</label>
                        <input type="text" id="land" v-model="editCamping.Land" required class="block w-2/3 border border-gray-300 rounded-md p-2">
                    </div>
                    <div class="flex items-center">
                        <label for="bescrijving" class="mr-2 w-1/3">Beschrijving:</label>
                        <textarea id="bescrijving" v-model="editCamping.Bescrijving" required class="block w-2/3 border border-gray-300 rounded-md p-2"></textarea>
                    </div>
                    <div class="flex justify-end space-x-2">
                        <button type="submit" class="inline-block bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">Update Camping</button>
                    </div>
                </form>
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