<template>
    <div>
        <h1>
            Account aanmaken
        </h1>
    
        <div>
            <form @submit.prevent="addUser">
                <div>
                    <label for="admin">Campingbeheerder:</label>
                    <input type="checkbox" id="admin" v-model="user.Admin">
                </div>
                <div>
                    <label for="name">Familinaam:</label>
                    <input type="text" id="name" v-model="user.Name" required>
                </div>
                <div>
                    <label for="first_name">Voornaam:</label>
                    <input type="text" id="first_name" v-model="user.First_Name" required>
                </div>
                <div>
                    <label for="mail">E-mail:</label>
                    <input type="email" id="mail" v-model="user.Mail" required>
                </div>
                <div>
                    <label for="password">Wachtwoord:</label>
                    <input type="password" id="password" v-model="user.Password" required>
                </div>
                <button type="submit">Maak account aan</button>
            </form>
        </div>
          
        <button @click="naarHome">Home</button>
    </div>
</template>

<script>
    export default {
        name : 'PageAdduser',
        data() {
            return {
                userId: null,
                isIngelogd: false,
                user: {
                    Admin: false,
                    Name: '',
                    First_Name: '',
                    Mail: '',
                    Password: ''
            }
            };
        },
        methods: {
            addUser() {
                fetch('http://localhost:3000/api/user', {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json'
                    },
                    body: JSON.stringify(this.user)
                })
                .then(response => response.json())
                .then(data => {
                    if (data) {
                        alert('Gebruiker toegevoegd!');
                        this.naarHome();
                    } else {
                        alert('Controleer je invoer.');
                    }
            })},
            naarHome() {
                window.location.href = '/';
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
        }
    }
</script>