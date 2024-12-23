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
                <button @click="showForm = false">Annuleer</button>
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
        name: 'PageUserInfo',
        data() {
            return {
                userId: null,
                isIngelogd: false,
                userInfo: [],
                toonVeranderGegevens: false,
                editUser: {}
            };
        },
        methods: {
            fetchUserInfo() {
                fetch("http://localhost:3000/api/user/" + this.userId)
                .then(response => response.json())
                .then(data => {
                    this.userInfo = data[0];
                    this.editUser = {...data[0]};
                    console.log(this.editUser);
                })
            },
            updateUserInfo() {
                fetch("http://localhost:3000/api/user/" + this.userId, {
                method: 'PUT',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify(this.editUser)
                })
                .then(response => response.json())
                .then(data => {
                    if (data.message) {
                        alert(data.message);
                        this.showForm = false;
                        this.fetchUserInfo(); // Refresh user info
                    } else {
                        alert('Failed to update user info');
                    }
            }).catch(error => {
                console.error('Error updating info:',error);
            })
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