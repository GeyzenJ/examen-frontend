<template>
    <div>
        <h1 class="font-bold text-[25px]">
            Gegevens gebruiker
        </h1>

        <div v-if="isIngelogd">
            <div v-for="(gegevens, index) in userInfo" :key="index" >   
                <p>Naam: {{ gegevens.First_Name }} {{ gegevens.Name }} </p>
                <p>E-mail: {{ gegevens.Mail }} </p>
                <p v-if="gegevens.Admin === 1">Rol: Campingbeheerder </p>
            </div>
            <div v-if="!toonVeranderGegevens">
                <p v-if="succesvolAangepast">Gegevens succesvol aangepast!</p>
                <button class="inline-block bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded" @click="toonVeranderGegevens = true">Wijzig gegevens</button>
            </div>
            <div v-if="toonVeranderGegevens">
                <h2 class="font-bold text-[15px]">Verander gebruiks gegevens</h2>
                <form @submit.prevent="updateUserInfo" class="space-y-4 bg-white p-6 rounded shadow-md w-full max-w-md mx-auto">
                    <div class="flex items-center">
                        <label for="first_name" class="mr-2 w-1/3">Voornaam:</label>
                        <input type="text" id="first_name" v-model="editUser.First_Name" required class="block w-2/3 border border-gray-300 rounded-md p-2">
                    </div>
                    <div class="flex items-center">
                        <label for="name" class="mr-2 w-1/3">Familienaam:</label>
                        <input type="text" id="name" v-model="editUser.Name" required class="block w-2/3 border border-gray-300 rounded-md p-2">
                    </div>
                    <div class="flex items-center">
                        <label for="mail" class="mr-2 w-1/3">E-mail:</label>
                        <input type="email" id="mail" v-model="editUser.Mail" required class="block w-2/3 border border-gray-300 rounded-md p-2">
                    </div>
                    <div class="flex justify-end space-x-2">
                        <button class="inline-block bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded mr-2" type="submit">Sla wijzigingen op</button>
                        <button class="inline-block bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded" @click="toonVeranderGegevens = false; succesvolAangepast = false">Annuleer</button>
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
        name: 'PageUserInfo',
        data() {
            return {
                userId: null,
                isIngelogd: false,
                userInfo: [],
                toonVeranderGegevens: false,
                succesvolAangepast: false,
                editUser: {}
            };
        },
        methods: {
            async fetchUserInfo() {
                const response =  await fetch("http://localhost:3000/api/user/" + this.userId);
                const data = await response.json();
                this.userInfo = data;
                this.editUser = data[0];
            },
            async updateUserInfo() {
                try {
                    const response = await fetch("http://localhost:3000/api/user/" + this.userId, {
                    method: 'PUT',
                    headers: {
                        'Content-Type': 'application/json'
                    },
                    body: JSON.stringify(this.editUser),
                    credentials: 'include' // Include credentials (cookies)
                    });
                    if (response.ok) {
                        this.toonVeranderGegevens = false;
                        this.succesvolAangepast = true;
                        this.fetchUserInfo();
                    } else {
                        console.log('Failed to update user!');
                        console.log(response);
                    }
                } catch (error) {
                    console.error(error);
                    }
            },
            checkLoginStatus() {
                this.userId = this.$cookies.get('userId');
                if (this.userId)
                {
                    this.isIngelogd = true;
                    this.fetchUserInfo();
                }
            }
        },
        created() {
            this.checkLoginStatus();
        }
    }
</script>