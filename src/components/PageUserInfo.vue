<template>
    <div>
        <h1>
            Gegevens gebruiker
        </h1>

        <div v-if="isIngelogd">
            <div v-for="(gegevens, index) in userInfo" :key="index" >   
                <p> {{ gegevens.First_Name }} {{ gegevens.Name }} </p>
                <p> {{ gegevens.Mail }} </p>
                <p v-if="gegevens.Admin === 1"> Campingbeheerder </p>
            </div>
            <div v-if="!toonVeranderGegevens">
                <p v-if="succesvolAangepast">Gegevens succesvol aangepast!</p>
                <button @click="toonVeranderGegevens = true">Wijzig gegevens</button>
            </div>
            <div v-if="toonVeranderGegevens">
                <h2>Verander gebruiks gegevens</h2>
                <form @submit.prevent="updateUserInfo">
                    <div>
                        <label for="first_name">Voornaam:</label>
                        <input type="text" id="first_name" v-model="editUser.First_Name" required>
                    </div>
                    <div>
                        <label for="name">Familienaam:</label>
                        <input type="text" id="name" v-model="editUser.Name" required>
                    </div>
                    <div>
                        <label for="mail">E-mail:</label>
                        <input type="email" id="mail" v-model="editUser.Mail" required>
                    </div>
                    <button type="submit">Sla wijzigingen op</button>
                    <button @click="toonVeranderGegevens = false; succesvolAangepast = false">Annuleer</button>
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