<template>
    <div>
        <h1 class="font-bold text-[25px]">
            Account aanmaken
        </h1>
    
        <div class="p-4">
            <form @submit.prevent="addUser" class="space-y-4 bg-white p-6 rounded shadow-md w-full max-w-sm mx-auto">
                <div class="flex items-center">
                    <label for="admin" class="mr-2">Campingbeheerder:</label>
                    <input type="checkbox" id="admin" v-model="user.Admin" class="mt-1">
                </div>
                <div class="flex items-center">
                    <label for="name" class="mr-2">Familinaam:</label>
                    <input type="text" id="name" v-model="user.Name" required class="mt-1 block w-1/2 border border-gray-300 rounded-md p-2">
                </div>
                <div class="flex items-center">
                    <label for="first_name" class="mr-2">Voornaam:</label>
                    <input type="text" id="first_name" v-model="user.First_Name" required class="mt-1 block w-1/2 border border-gray-300 rounded-md p-2">
                </div>
                <div class="flex items-center">
                    <label for="mail" class="mr-2">E-mail:</label>
                    <input type="email" id="mail" v-model="user.Mail" required class="mt-1 block w-1/2 border border-gray-300 rounded-md p-2">
                </div>
                <div class="flex items-center">
                    <label for="password" class="mr-2">Wachtwoord:</label>
                    <input type="password" id="password" v-model="user.Password" required class="mt-1 block w-1/2 border border-gray-300 rounded-md p-2">
                </div>
                <button class="mt-4 bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded" type="submit">Maak account aan</button>
            </form>
            <button class="mt-4 bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded" @click="naarHome">Home</button>
        </div>
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
                        alert('Het is niet gelukt om je als gebruiker toe te voegen.');
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