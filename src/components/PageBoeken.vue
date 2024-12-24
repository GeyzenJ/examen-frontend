<template>
    <div>
        <h1>
            Camping boeken
        </h1>

        <div v-if="isIngelogd">
            <h2>
                Boek een camping
            </h2>
            <form @submit.prevent="bookCamping">
                <div>
                    <label for="camping">Camping:</label>
                    <select v-model="booking.campingId" required>
                        <option v-for="(camping, index) in campings" :key="index" :value="camping.ID">{{ camping.Naam }}</option>
                    </select>
                </div>
                <div>
                    <label for="startDatum">Start Datum:</label>
                    <input type="date" v-model="booking.startDatum" required>
                </div>
                <div>
                    <label for="eindDatum">Eind Datum:</label>
                    <input type="date" v-model="booking.eindDatum" required>
                </div>
                <div>
                    <label for="electriciteit">Electriciteit:</label>
                    <input type="checkbox" v-model="booking.electriciteit">
                </div>
                <button type="submit">Boek</button>
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