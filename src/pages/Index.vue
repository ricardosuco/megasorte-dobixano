<template>
  <div class="fonts">
    <header class="flex justify-center items-end shadow-4 print-hide" style="min-height: 40vh; background: url('images/bg3.jpg') center no-repeat; background-size: cover">
      <div class="row flex justify-center q-gutter-sm full-width">
        <div class="col-xs-4 col-sm-2 col-md-1 text-center">
        <label class="text-weight-bold text-white" for="">
          Qnt de jogos
           <q-tooltip anchor="top middle">
            <span class="text-subtitle2">Quantidade de jogos a serem gerados</span>
            </q-tooltip>
        </label>
        <q-input color="teal-10" bg-color="white" v-model="orders" dense mask="###" type="number" outlined/>
        </div>
        <div class="col-xs-4 col-sm-2 col-md-1 text-center">
        <label class="text-weight-bold text-white" for="">
            Qnt de dezenas
            <q-tooltip anchor="top middle">
            <span class="text-subtitle2">A quantidade de dezenas por jogos precisa ser entre 6 e 20</span>
            </q-tooltip>
        </label>
        <q-input color="teal-10" bg-color="white" v-model="amountOfDozens" dense type="number" min=6 max=20 outlined/>
        </div>
        <div class="col-xs-4 col-sm-2 col-md-1 text-center">
        <label class="text-weight-bold text-white" for="">
          Dezena da sorte
           <q-tooltip anchor="top middle">
            <span class="text-subtitle2">Aqui você insere sua dezena da sorte para ser incluida nos jogos</span>
            </q-tooltip>
        </label>
        <q-input color="teal-10" bg-color="white" v-model="luckDozen" dense min=1 type="number" max=60 outlined/>
        </div>
      </div>
        <div class="row q-mb-xl">
          <div class="flex column items-center">
            <q-toggle color="white" class="q-mb-md" v-model="addictedDozens" label="" size="lg">
              <q-tooltip anchor="top middle" class="text-subtitle2">{{addictedDozens ? 'Sortear entre as dezenas que mais sairam na história da mega da virada' : 'Sortear entre 01 e 60'}}</q-tooltip>
            </q-toggle>
            <q-btn class="q-px-xl" @click="getTicket" color="light-green" text-color="white" size="lg" :disable="!orders" no-caps>Sortear</q-btn>
          </div>
        </div>
    </header>
    <section style="padding: 40px; color: #f4f4f4">
    <div class="q-gutter-md flex justify-center">
      <q-card flat bordered style="max-width: 270px" v-for="ticket of tickets" :key="ticket.order">
        <q-card-section class="flex justify-between items-center bg-green-8">
          <div class="text-subtitle2 text-white">Bilhete Nº {{ticket.order}}</div>
          <div>
            <q-icon name="fa-solid fa-clover" color="light-green" size="sm"/>
          </div>
        </q-card-section>
          <q-separator />
        <q-card-section class="flex">
            <div class="q-gutter-sm">
              <q-avatar v-for="(item, index) of ticket.ticket" :key="index" size="md" color="light-green" text-color="white">{{item}}</q-avatar>      
          </div>
        </q-card-section>
      </q-card>
    </div>
    </section>
    <q-page-sticky class="print-hide" :offset="[12, 100]" position="bottom-right" style="z-index: 1000">
    <q-btn v-show="showPrint" rounded color="light-green" @click="copyToTransferArea" icon="content_copy"> 
      <q-tooltip class="text-subtitle2">Copiar para área de transferência</q-tooltip>
    </q-btn>   
    </q-page-sticky>
    <q-page-sticky class="print-hide" :offset="[12, 55]" position="bottom-right" style="z-index: 1000">
    <q-btn v-show="showPrint" rounded color="green" @click="exportCsv" icon="file_download"> 
      <q-tooltip class="text-subtitle2">Exportar CSV</q-tooltip>
    </q-btn>   
    </q-page-sticky>
    <q-page-sticky class="print-hide" :offset="[12, 12]" position="bottom-right" style="z-index: 1000">
    <q-btn v-show="showPrint" rounded color="blue" @click="print" icon="print"> 
      <q-tooltip class="text-subtitle2">Imprimir</q-tooltip>
    </q-btn>   
    </q-page-sticky>
    <q-page-sticky class="print-hide" :offset="[12, 12]" position="bottom-left" style="z-index: 1000">
    <q-btn rounded color="blue" @click="showModal = true" icon="help"> 
      <q-tooltip class="text-subtitle2">Ajuda</q-tooltip>
    </q-btn>   
    </q-page-sticky>
    <q-dialog v-model="showModal">
      <q-card>
        <q-card-section>
            <q-btn class="absolute-top-right" rounded flat v-close-popup icon="close" />
          <div class="text-center">
            <span class="text-h6 text-grey-9">Instruções de uso</span>
          </div>
        </q-card-section>
        <q-card-section class="row items-center bg-light-green">
          <div class="q-gutter-md flex text-subtitle1 text-white">
            <span><b>Qnt de jogos</b>: A quantidade de jogos que você deseja sortear. Quantidade máxima: 200 por vez</span>
            <span><b>Qnt de dezenas</b>: A quantidade de dezenas que você deseja que seus jogos tenham. A megasena permite de 6 a 20</span>
            <span><b>Dezena da sorte</b>: Você pode incluir uma dezena entre 01 e 60, que julgue ser seu numero da sorte, e ela aparecerá em todos os jogos</span>
            <span>O botão do tipo<b> toggle</b> no centro seleciona se os numeros serão sorteados entre todas as dezenas da megasena ou entre as que mais foram sorteadas na história da mega da virada</span>
            <span>Após sortear, no canto inferior direito aparecerá botões com as seguintes ações: Copiar os bilhetes para área de tranferência, exportar csv e imprimir página</span>
            <div class="column text-center text-caption" style="margin: 8px auto">
              <span>Quando ganhar não esquece o pix do pai </span>
              <span>81e8b2cb-ed67-433b-afb7-789fe8b6d380</span>
              <span class="text-subtitle1">&#128527;</span>
            </div>
          </div>
        </q-card-section>
      </q-card>
    </q-dialog>
  </div>
</template>

<script>
// const body = document.getElementsByTagName('body')[0]
// body.addEventListener("mouseleave", () => {
//   alert("Quando ganhar não esquece o pix do pai :D \n\n 81e8b2cb-ed67-433b-afb7-789fe8b6d380")
// });
import { defineComponent } from 'vue';
const addictedDozens = [
  4, 5, 8, 10, 11, 13, 16, 17, 18, 23, 24, 27, 28, 29, 30, 32, 33, 34, 35, 36,
  37, 38, 41, 42, 43, 44, 49, 51, 53, 54, 56,
];

const allDozens = [
  1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22,
  23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40, 41, 42,
  43, 44, 45, 46, 47, 48, 49, 50, 51, 52, 53, 54, 55, 56, 57, 58, 59, 60,
];

export default defineComponent({
  name: 'PageIndex',

    data() {
      return {
        orders: 1,
        tickets: [],
        amountOfDozens: 6,
        luckDozen: '',
        addictedDozens: true,
        showModal: false,
      }
    },

    watch: {
      amountOfDozens(newValue) {
        if (newValue < 6 || newValue > 20) {
          this.amountOfDozens = 6;
          this.$q.notify({
            message: 'A quantidade de dezenas por orders precisa ser entre 6 e 20',
            color: 'red',
            position: 'top',
            timeout: 2000,
          });
        }
      },
      luckDozen(newValue) {
        if (newValue !== '' && (newValue < 1 || newValue > 60)) {
          this.luckDozen = '';
          this.$q.notify({
            message: 'A dezena da sorte precisa ser entre 1 e 60',
            color: 'red',
            position: 'top',
            timeout: 2000,
          });
        }
      },
    },
    computed: {
      showPrint() {
        return this.tickets.length > 0;
      },
    },
    methods: {
        getTicket() {
          if (this.orders > 200) {
            this.$q.notify({
              message: 'Não me trave... só 200 orders por vez!',
              color: 'negative',
              position: 'top',
              timeout: 2000
            })
            return
          }
          this.$q.loading.show({
            html: true,
            message: 
              '<span class="text-weight-bold text-subtitle1">Mentalizando os números da sorte...</span><img style="max-width: 350px; max-height: 300px; border-radius: 50%" src="images/luckcat.jpg"/>',
            spinner: false,
          });
          setTimeout(() => {
        let dozensDrawn = [];
    
        for(let i = 0; i < this.orders; i++) {
            let arrDozens = [];
            this.luckDozen && arrDozens.push(this.luckDozen);
            let dozens = this.addictedDozens ? addictedDozens : allDozens;
            while(arrDozens.length < this.amountOfDozens) {
                let randomDozen = Math.floor(Math.random() * dozens.length);
                if(!arrDozens.includes(dozens[randomDozen])) {
                    arrDozens.push(dozens[randomDozen] > 9 ? dozens[randomDozen] : `0${dozens[randomDozen]}`);
                }
            }
            arrDozens.sort((a, b) => a - b);
            dozensDrawn.push({
                order: i + 1,
                ticket: arrDozens
            });
            this.tickets = [...dozensDrawn]
        }
        this.$q.loading.hide()
          }, 2000);
      },

      print() {
        window.print()
      },

      exportCsv() {
        let csv = this.tickets.map((item, index) => {
          return 'Bilhete N #' + (index + 1) + ';' + item.ticket.join('-')
        }).join('\n');
        const blob = new Blob([csv], { type: 'text/csv' });
        const url = window.URL.createObjectURL(blob);
        const link = document.createElement('a');
        link.setAttribute('href', url);
        link.setAttribute('download', 'tickets.csv');
        link.click();
      },
      
      copyToTransferArea() {
        let text = this.tickets.map((item, index) => {
          return `Bilhete Nº #${index + 1} - [${item.ticket.join(' - ')}]`
        }).join('\n');
        navigator.clipboard.writeText(text);
        this.$q.notify({
          message: 'Copiado para área de transferência',
          color: 'green',
          position: 'top',
          timeout: 2000,
        });
      },
  },

})
</script>
<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700;900&display=swap');
.fonts {
  font-family: 'Poppins', sans-serif;
}

label {
  font-size: 13px;
}
</style>
