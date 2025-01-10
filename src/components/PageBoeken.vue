<template>
    <div>
        <h1 class="font-bold text-[25px]">
            Camping boeken
        </h1>

        <div v-if="isIngelogd">
            <h2 class="font-bold text-[15px]"> 
                Boek een camping
            </h2>

            <form @submit.prevent="bookCamping" class="space-y-4 bg-white p-6 rounded shadow-md w-full max-w-md mx-auto">
                <div class="flex items-center">
                    <label for="camping" class="mr-2 w-1/3">Camping:</label>
                    <select v-model="booking.campingId" required class="block w-2/3 border border-gray-300 rounded-md p-2">
                        <option v-for="(camping, index) in campings" :key="index" :value="camping.ID">{{ camping.Naam }}</option>
                    </select>
                </div>
                <div class="flex items-center">
                    <label for="startDatum" class="mr-2 w-1/3">Start Datum:</label>
                    <input type="date" v-model="booking.startDatum" required class="block w-2/3 border border-gray-300 rounded-md p-2">
                </div>
                <div class="flex items-center">
                    <label for="eindDatum" class="mr-2 w-1/3">Eind Datum:</label>
                    <input type="date" v-model="booking.eindDatum" required class="block w-2/3 border border-gray-300 rounded-md p-2">
                </div>
                <div class="flex items-center">
                    <label for="electriciteit" class="mr-2 w-1/3">Electriciteit:</label>
                    <input type="checkbox" v-model="booking.electriciteit" class="mt-1">
                </div>
                <div class="flex justify-end space-x-2">
                    <button type="submit" class="inline-block bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">Boek</button>
                </div>
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
        name: 'PageBoeken',
        data() {
            return {
                userId: null,
                isIngelogd: false,
                campings: [],
                booking: {
                    campingId: null,
                    startDatum: '', 
                    eindDatum: '',
                    electriciteit: false
                }
            };
        },
        methods: {  
            async fetchCampings() {
                const response =  await fetch("http://localhost:3000/api/campings");
                const data = await response.json();
                this.campings = data;
            },
            async bookCamping() {
            try {
                    const vandaag = new Date().toISOString().split('T')[0];
                    if (this.booking.startDatum < vandaag) {
                        alert('Begin datum kan niet in het verleden liggen.');
                    }
                    const response = await fetch("http://localhost:3000/api/book", {
                        method: 'POST',
                        headers: {
                            'Content-Type': 'application/json'
                        },
                        body: JSON.stringify({ ...this.booking, userId: this.userId }),
                        credentials: 'include'
                    });
                    if (response.ok) {
                        alert('Booking aangemaakt!');
                    } else {
                        const errorData = await response.json();
                        alert(`Kan boeking niet plaatsen: ${errorData.message}`);
                    }
                } catch (error) {
                    console.error('Error creating booking:', error);
                }
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