<template>
    <div class="wrapper">
        <form @submit.prevent="handleSubmit" class="form">
            <div class="form__content">
                <span>Imię:</span>
                <input type="text" class="form__content-input" />
            </div>
            <div class="form__content">
                <span>Nazwisko:</span>
                <input type="text" class="form__content-input" />
            </div>
            <div class="form__content">
                <span>E-mail:</span>
                <input type="text" class="form__content-input" />
            </div>
            <div id="example-container" class="cf-turnstile"></div>
            <div v-if="refreshing" class="refresh-message">Refreshing...</div>

            <button class="form__content-button" :disabled="!challengeAccepted">
                Submit
            </button>
        </form>
    </div>
</template>

<script>
export default {
    data() {
        return {
            showContainer: false,
            challengeAccepted: false,
            refreshing: false,
        };
    },
    mounted() {
        this.loadTurnstileScript();
    },
    methods: {
        loadTurnstileScript() {
            const script = document.createElement("script");
            script.src =
                "https://challenges.cloudflare.com/turnstile/v0/api.js?onload=onloadTurnstileCallback";
            script.defer = true;

            script.onload = () => {
                window.onloadTurnstileCallback = () => {
                    turnstile.render("#example-container", {
                        sitekey: "0x4AAAAAAAKIG2pCBqUdLMno",
                        callback: (token) => {
                            console.log(`Challenge Success ${token}`);
                            this.challengeAccepted = true;
                        },
                    });

                    this.showContainer = true;
                };
            };

            document.head.appendChild(script);
        },
        handleSubmit() {
            this.refreshing = true;
            setTimeout(() => {
                this.refreshPage();
            }, 2000);
        },

        refreshPage() {
            window.location.reload();
        },
    },
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Cinzel:wght@700&family=Montserrat:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&display=swap");
body {
    font-family: "Cinzel", serif;
    font-family: "Montserrat", sans-serif;
}
.refresh-message {
    font-size: 20px;
    margin-top: 10px;
}
.wrapper {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    height: 80vh;
}
.form {
    font-size: 24px;
    &__content {
        display: flex;
        flex-direction: column;
        padding-bottom: 30px;
        &-input {
            font-size: 24px;
        }
        &-button {
            width: 100%;
            margin-top: 20px;
            padding: 5px;
            font-size: 20px;
        }
    }
}
</style>
