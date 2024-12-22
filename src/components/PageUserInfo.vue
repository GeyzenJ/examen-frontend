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
        </div>

        <div v-if="!isIngelogd">
            <p>
                niet ingelogd!
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
                userInfo: []
            };
        },
        methods: {
            fetchUserInfo() {
                fetch("http://localhost:3000/api/user/" + this.userId)
                .then(response => response.json())
                .then(data => {
                    this.userInfo = data;
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