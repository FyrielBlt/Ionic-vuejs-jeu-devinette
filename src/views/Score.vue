<template>
  <ion-page>
    <ion-header translucent>
        <ion-toolbar color="primary"> 
            <ion-title>
                  <ion-buttons>
                <ion-back-button default-href="home"></ion-back-button>les scores des joueurs
              </ion-buttons>  
            </ion-title>
        </ion-toolbar>
    </ion-header> 
    <ion-content :fullscreen="true">
  <ion-card id="container">
  <form>
         <ion-item  v-for="(joueur,index) in joueurs" :key="index">
          place: {{index}} {{joueur.nom }} score= {{ joueur.score }} temps emis : {{ joueur.timing }}
        </ion-item>      
              </form>
      <ion-card-content>   
      </ion-card-content>
  </ion-card>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import {IonContent, IonHeader, IonPage, IonItem, IonCard, IonCardContent,IonBackButton} from '@ionic/vue';
import { defineComponent } from 'vue';
import { addIcons } from "ionicons";
import { image } from "ionicons/icons";
import axios from 'axios';
addIcons({
  "image": image
})
export default defineComponent({
  data() {
    return {
      joueurs:[]
    }
  },
  created() {
       axios.get('http://localhost:3000/devinette?_sort=score,timing&_order=desc,asc')
                    .then(response=>this.joueurs=response.data)
    
  }, 
  name: 'Home',
  components: {
    IonContent,
    IonHeader,
    IonPage,
    IonItem,
    IonCard, IonCardContent,IonBackButton
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
