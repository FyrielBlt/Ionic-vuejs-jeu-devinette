<template>
  <ion-page>
    <ion-header translucent>
        <ion-toolbar color="primary"> 
            <ion-title>
               Jeu Devinette  <ion-label :hidden="hiddenvalue" >time:{{time}}</ion-label>
            </ion-title>

        </ion-toolbar>
    </ion-header> 
    <ion-content :fullscreen="true">
  <ion-card id="container">
  <form>
  pour tester voila le numero a deviner {{answer}} <br>
   {{res}}
        <ion-item>
          <ion-label position="floating"> taper pour deviner un random number
          </ion-label>
          <ion-input :disabled="disabledvalue" v-model="randomnbr"></ion-input>
        </ion-item>
         <ion-button expand="block" :disabled="disabledvalue"  @click="deviner">Deviner</ion-button>
         <ion-button expand="block" :disabled="startvalue" @click="restart">start</ion-button>
         <ion-button expand="block" router-link="/score">Voir scores</ion-button>
        </form>
         <ion-item  v-for="his in historique" :key="his">
           {{his}}
     </ion-item>
      <ion-card-content>   
      </ion-card-content>
  </ion-card>
    </ion-content>
  </ion-page>
</template>
<script lang="ts">
import {IonContent, IonHeader, IonPage, IonItem, IonLabel,
 IonInput, IonButton, IonCard, IonCardContent,alertController } from '@ionic/vue';
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
       randomnbr:'' as any,  
       answer: '' as any,
       res:'' as any, 
       tentative:5,  
       nbrtentative:0,
       score :0,
       condition:'',
       disabledvalue:true,
       historique:[] as any,
       nom:'',
       hiddenvalue:true,
       time:0,
       timetaken:0,
       startvalue:false,
    }
  },
  methods: {  
      restart(){
        if(this.nbrtentative<5 ){
           this.disabledvalue=false;
           this.startvalue=true;
              this.hiddenvalue=false;
         setInterval(() => {
          this.time += 1;
      }, 1000);
           this.answer=Math.floor(Math.random() * (100 - 1 + 1)) + 1 ;
               }
               else{
                 window.location.reload();
               }
      },
    async  deviner(){
          if(this.tentative>0){
           if(this.randomnbr==this.answer){
             this.timetaken=this.time;
            this.score = (5 - this.nbrtentative +1) * 10;
            this.res='bravo votre score est : '+this.score;
             const alert = await alertController.create({
        header: 'temps émis:'+this.timetaken,
        message:this.res+' ecrivez votre nom pour enregistrer le score',
        inputs: [
        {
          name: 'name',
          type: 'text'
        }],    
         buttons: [
          {
              text: 'Ok',
              handler: (alertData) => { 
                this.nom=alertData.name;
                axios.post('http://localhost:3000/devinette',{
                  nom:this.nom,
                  score:this.score,
                  timing:this.timetaken,
                 })
            }
            }
          ]
      });
      await alert.present();
               
           }
            else{
            if(this.randomnbr<this.answer)
           this.condition=' le nombre que vous avez fournit est plus petit';
           else
           this.condition=' le nombre que vous avez fournit est plus grand';
           this.res='échoué'+this.condition;
           this.tentative=this.tentative-1;
           this.historique[this.nbrtentative]=" tentative numero: "+this.nbrtentative+" le chiffre proposé est :"+this.randomnbr+" Message : "+this.condition+"temps émis :"+this.time ; 
           this.nbrtentative=this.nbrtentative+1;
               }
      }
         if(this.tentative==0){
             this.res=' Max tentative  atteint votre score est : 0';
                          this.disabledvalue=true;

         }
      }
  }, 
  name: 'Home',
  components: {
    IonContent,
    IonHeader,
    IonPage,
    IonItem,
    IonInput,
    IonLabel, IonButton, IonCard, IonCardContent,
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