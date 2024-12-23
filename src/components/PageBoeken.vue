<template>
    <div>
        <h1>
            Camping boeken
        </h1>

        <div v-if="isIngelogd">
            <p>
                Je bent ingelogd!
            </p>
            <ul v-for="(camping, index) in campings" :key="index">
                <li>
                    <ul>
                        <li>
                            {{ camping.Naam }}
                        </li>
                    </ul>
                </li>
            </ul>
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
        name: 'PageBoeken',
        data() {
            return {
                userId: null,
                isIngelogd: false,
                campings: []
            };
        },
        methods: {
            async fetchCampings() {
                const response =  await fetch("http://localhost:3000/api/campings");
                const data = await response.json();
                this.campings = data;
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
            this.fetchCampings();
        }
    }
</script>