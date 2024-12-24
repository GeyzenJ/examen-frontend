<template>
    <div>
        <h1>
            Gegevens gebruiker
        </h1>

        <div v-if="isIngelogd">
            <p>Hoi je ben tingeldodhg </p>
            
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
        name: 'PageUpdateCamping',
        data() {
            return {
                userId: null,
                isIngelogd: false,
                toonVeranderGegevens: false,
                succesvolAangepast: false,
                editCamping: []
            };
        },
        methods: {
            async updateCamping() {
                try {
                    const response = await fetch("" + this.userId, {
                    method: 'PUT',
                    headers: {
                        'Content-Type': 'application/json'
                    },
                    body: JSON.stringify(this.editUser),
                    credentials: 'include' // Include credentials (cookies)
                    });
                    if (response.ok) {
                        //Camping shizzel
                    } else {
                        console.log('Failed to update Camping!');
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
                }
            }
        },
        created() {
            this.checkLoginStatus();
        }
    }
</script>