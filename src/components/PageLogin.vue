<template>
    <div>
        <h1 class="font-bold text-[25px]">
            Login
        </h1>
        <div v-if="!isIngelogd">
            <p class="p-[25px]">Log in met je e-mail en wachtwoord</p>

            <div class="p-4">
                <form @submit.prevent="login" class="space-y-4 bg-white p-6 rounded shadow-md w-full max-w-sm mx-auto">
                    <input v-model="mail" placeholder="Mail" required class="block w-full border border-gray-300 rounded-md p-2 mb-4" />
                    <input v-model="password" type="password" placeholder="Wachtwoord" required class="block w-full border border-gray-300 rounded-md p-2 mb-4" />
                    <button type="submit" class="w-full bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">Login</button>
                </form>
            </div>
        </div>

        <div v-if="isIngelogd">
            <p class=p-[25px]>
                Login succesvol!
            </p>
            <button class="inline-block bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded" @click="logout" >Log uit</button>
        </div>
        
    </div>
</template>

<script>
    export default {
        name: 'PageLogin',
        data() {
            return {
                mail: '',
                password: '',
                userId: null,
                isIngelogd: false,
                isAdmin: false
            };      
        },
        methods: {
            async login() {
                try {
                    const response = await fetch('http://localhost:3000/api/login', {
                        method: 'POST',
                        headers: {
                            'Content-Type': 'application/json'
                        },
                        body: JSON.stringify({
                            mail: this.mail,
                            password: this.password
                        }),
                        credentials: 'include' // Include credentials (cookies)
                    });
                    if (response.ok) {
                        this.userId = this.$cookies.get('userId');
                        this.isAdmin = this.$cookies.get('isAdmin');
                        this.$emit('login', this.userId, this.isAdmin);
                        this.isIngelogd = true
                    } else {  
                        alert("Login mislukt: Controleer E-mail en wachtwoord!");
                    }                  
            } catch (error) {
                console.error(error);
                }
            },
            async logout() {
                const response = await fetch('http://localhost:3000/api/logout', {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json'
                    },
                    credentials: 'include' //!belagnrijk voor cookie
                });
                if (response.ok) {
                    this.isIngelogd = false;
                    this.userId = null; //!userId terug op null
                    this.$emit('logout');
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
        }
    }
</script>