<template>
    <div>
        <h1>
            Login
        </h1>
        
        <p>Log in met je e-mail en wachtwoord</p>

        <div>
            <form @submit.prevent="login">
                <input v-model="mail" placeholder="Mail" required />
                <input v-model="password" type="password" placeholder="Wachtwoord" required />
                <button type="submit">Login</button>
            </form>
        </div>
       
    </div>
</template>

<script>
    export default {
        name: 'PageLogin',
        data() {
            return {
                mail: '',
                password: ''
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
                        })
                    });
                    const data = await response.json();
                    if (response.ok) {
                        console.log('logins succesvol', data);
                    } else {
                        console.error('login fail', data);
                    }
                                
            } catch (error) {
                console.error(error);
      }
            }
        }
    }
    
</script>