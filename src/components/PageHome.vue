<template>
    <div>

        <div v-if="!addAccount">
            <h1>
            Camping Reserveren
        </h1>
        <p>
            Op deze webiste kan je een campingplaats reserveren.
        </p>
        <div v-if="!isIngelogd">
            <button @click="changeToCreateAccount('PageAddUser')">Account Aanmaken</button>
        </div>
        

        </div>
        
        <div>
            <PageAddUser v-if="activePage == 'PageAddUser'" />
        </div>
    </div>
</template>

<script>
import PageAddUser from './PageAddUser.vue';

    export default {
        name : 'PageHome',
        components: {
            PageAddUser
        },
        data() {
            return {
                userId: null,
                isIngelogd: false,
                activePage: '',
                addAccount: false
            };
        },
        methods: {
            changeToCreateAccount(page) {
                this.activePage = page;
                this.addAccount = true;
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