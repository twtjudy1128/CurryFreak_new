<template>
  <section class="section">
   <client-only>
    <div class="columns is-multiline">
      <div v-for="(data,key) in allData" :key="key" class="column is-4">
        <card
           v-bind:title="data.title"
           v-bind:url="data.url"
        >
          名前：{{data.name}}<br>
          ひとこと：{{data.memo}}<br>
        </card>
      </div>
    </div>
   </client-only>
  </section>
</template>

<script>
import Card from '~/components/Card'
import firebase from "firebase/app";
import "firebase/firestore";
export default {
  components: {
    Card
  },
  name: 'HomePage',
  data(){
    return{
      allData:[]
      
    }
  },
  methods: {
    //初期化、設定
    init: () => {
          // 自分の情報に置き換え
        const config = {
            apiKey: "自分の情報",
            authDomain: "自分の情報",
            databaseURL: "自分の情報",
            projectId: "自分の情報",
            storageBucket: "自分の情報",
            messagingSenderId: "自分の情報",
            appId: "自分の情報"
        };
        // Initialize Firebase
            if (firebase.apps.length === 0) {
            firebase.initializeApp(config);
            }
    },
    
    
    //データ取得
    get: function(){
        this.allData = [];
        firebase.firestore().collection('memos').get().then(snapshot => {
            snapshot.forEach(doc => {
                // console.log(doc);
                this.allData.push(doc.data());
            })
        });
    }
  },
    mounted(){
        //ページ読み込み時に実行される
        this.init();
        this.get();
    },
}
</script>
