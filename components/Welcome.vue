<template>
    <div>
        <div class="welcome_container" :style="{ backgroundImage: `url(${pageBackgroundImage})` }">
            <div v-if="showWelcome" :key="'loading'" class="welcome_container">
                <div v-if="loading">
                    <p :style="{ color: secondaryColor }">Yükleniyor...</p>
                </div>

                <div v-else class="text-center">
                    <div v-if="logo != null"  class="logo_container text-center">
        
        <img :src="logo" class="img-fluid" height="300" width="300" alt="">
      </div>
                    <h1 :style="{ color: primaryColor }">{{ welcomeMessage.title }}</h1>
                    <p :style="{ color: primaryColor }">{{ welcomeMessage.description }}</p>
                    <button class="btn" :style="{ backgroundColor: secondaryColor, borderColor: secondaryColor }"
                        @click="startForm">Başla</button>

                </div>


            </div>
            <div class="page_details ">
        <h5 :style="{ color: tertiaryColor }"> {{ pageTitle }}</h5>
        <p :style="{ color: tertiaryColor }"> {{ pageDesc }}</p>
      </div>
        </div>
    </div>
</template>



<script>

 export default {
    props: {
        showWelcome: {
            type: Boolean,
            default: true
        },
        secondaryColor: {
            type: String,
        },
        primaryColor: {
            type: String,
        },
        welcomeMessage: {
            type: Object,
        },
        logo: {
            type: String,
        },
        loading: {
            type: Boolean,
        },
        pageTitle: {
            type: String,
        },
        pageDesc: {
            type: String,
        },
        tertiaryColor: {
            type: String,
        },
        pageBackgroundImage: {
            type: String,
        }
    },

    data() {
        return {
        }
    },
    methods: {
        startForm() {
            this.$emit('startForm');
        }
    }
}

</script>

<style scoped>

.welcome_container {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    background-position: center;
    background-size: cover;
}

.page_details {
  position: absolute;
  bottom: 0;
  left: 0;
  padding: 20px;
  text-align: left;
}
</style>