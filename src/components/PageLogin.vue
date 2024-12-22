<template>
    <div>
        <h1>
            Login
        </h1>
        <div v-if="!isIngelogd">
            <p>Log in met je e-mail en wachtwoord</p>

            <div>
                <form @submit.prevent="login">
                    <input v-model="mail" placeholder="Mail" required />
                    <input v-model="password" type="password" placeholder="Wachtwoord" required />
                    <button type="submit">Login</button>
                </form>
            </div>
        </div>

        <div v-if="isIngelogd">
            <p>
                Login succesvol!
            </p>
            <button @click="logout">Log uit</button>
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
                isIngelogd: false
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
                        this.isIngelogd = true
                    } else {
                        const data = await response.json();
                        console.error('Login failed', data);
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