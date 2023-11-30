<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-buttons slot="primary">
          <ion-button color="primary" fill="solid" @click="info">
            <ion-icon :icon="infoIcon"></ion-icon>
          </ion-button>
        </ion-buttons>
        <ion-title>Comptador</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true">
      <ion-header class="ion-no-border ion-padding-top ion-padding-horizontal">
        <ion-grid>
          <ion-row>
            <ion-col>
              <div class="ion-text-start">
                Puntuació: {{ score }}
              </div>
            </ion-col>

            <ion-col>
              <div class="ion-text-end">
                Temps restant: {{ timeLeft }}
              </div>
            </ion-col>

          </ion-row>

        </ion-grid>

      </ion-header>
      <div id="container">
        <ion-button id="tapMeButton" color="primary" fill="solid" @click="tap">
          Tocam
        </ion-button>
      </div>
    </ion-content>
  </ion-page>
</template>

<script>
import {
  IonContent,
  IonHeader,
  IonPage,
  IonTitle,
  IonToolbar,
  IonButtons,
  IonButton,
  IonIcon,
  alertController,
  IonGrid,
  IonRow,
  IonCol, toastController, createAnimation,

} from '@ionic/vue';
import { informationCircleOutline } from "ionicons/icons";
import { defineComponent } from "vue";

const INITIAL_TIME = 5
export default defineComponent({
  name: 'Home',
  components: {
    IonContent,
    IonHeader,
    IonPage,
    IonTitle,
    IonToolbar,
    IonButtons,
    IonButton,
    IonIcon,
    IonGrid,
    IonRow,
    IonCol
  },
  setup () {
    return {
      infoIcon: informationCircleOutline,
      started: false
    }
  },
  data () {
    return {
      score: 0,
      timeLeft: INITIAL_TIME,
      counterInterval: null
  }
  },
  watch: {
    timeLeft: function(newTimeLeft) {
      if (newTimeLeft <= 0) {
        this.started = false
        this.timeLeft = INITIAL_TIME
        clearInterval(this.counterInterval)
        this.showResult()
        this.score = 0
      }
    }
  },
  methods: {
    bounce() {
      const animation = createAnimation()
      animation.addElement(document.getElementById('tapMeButton'))
          .duration(200)
          .fromTo('transform','scale(1.2)','scale(1.0)')
      animation.play();
    },
    async info () {
      const alert = await alertController
          .create({
            header: 'Comptador 1.0',
            subHeader: 'Creat per Andreu Gisbert Bel',
            message: 'Github: https://github.com/agisbertb/ComptadorIonic',
            buttons: ['OK'],
          });
      await alert.present();
      },
    async tap () {
      this.bounce()
      this.score++
      if (!this.started) {
        this.counterInterval = setInterval(() => {
          this.timeLeft--
        }, 1000)
        this.started = true
      }
    },
    async showResult(){
      const toast = await toastController.create({
        color: 'dark',
        duration: 2000,
        message: `Temps acabat. La teva puntuació és: ${this.score}`,
        showCloseButton: true
      });
      await toast.present()
    },
    }
});

</script>



<style scoped>
#container {
  text-align: center;
  
  position: absolute;
  left: 0;
  right: 0;
  top: 50%;
  transform: translateY(-50%);
}

#container strong {
  font-size: 20px;
  line-height: 26px;
}

#container p {
  font-size: 16px;
  line-height: 22px;
  
  color: #8c8c8c;
  
  margin: 0;
}

#container a {
  text-decoration: none;
}
</style>
