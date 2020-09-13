<template>
  <section class="section">
    　<div class="postform">
        <div>
          <input v-model="title" placeholder="店名"><br>
          <input v-model="name" placeholder="名前"><br>
          <input v-model="memo" placeholder="ひとこと"><br>
          <input v-show="!image_url" type="file" id="image_file" @change="onFileChange"  accept="image/*" required/>
          <b-button type="is-warning" v-on:click='post' v-show="show"><b>投稿</b></b-button>
        </div>
      </div>

  </section>
</template>

<script>
import firebase from "firebase/app";
import "firebase/firestore";
import 'firebase/storage';
export default {
  name: 'HomePage',
  props: {
    api: {
      type: Object,
      url: String,
      token: String,
      service: String
    }
  },
  data: function(){
    return {
      title:'',
      name:'',
      memo:'',
      downloadURL:'',
      show: false,
    };
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
        onFileChange: async function (e) {
            let files = e.target.files || e.dataTransfer.files;
            if (!files.length) {
              return;
            }
            const file = e.target.files[0];
            const storageRef = firebase.storage().ref(file.name);
            //Firebaseストレージに保存
            const uploadTask = storageRef.put(file);
            const _this = this;
              uploadTask.on(
                "state_changed",
                function (snapshot) {
                  // Observe state change events such as progress, pause, and resume
                  // Get task progress, including the number of bytes uploaded and the total number of bytes to be uploaded
                  var progress =
                    (snapshot.bytesTransferred / snapshot.totalBytes) * 100;
                  console.log("Upload is " + progress + "% done");
                  switch (snapshot.state) {
                    case firebase.storage.TaskState.PAUSED: // or 'paused'
                      console.log("Upload is paused");
                      break;
                    case firebase.storage.TaskState.RUNNING: // or 'running'
                      console.log("Upload is running");
                      break;
                  }
                },
                function (error) {
                  // Handle unsuccessful uploads
                },
                function () {
                  // Handle successful uploads on complete
                  // For instance, get the download URL: https://firebasestorage.googleapis.com/...
                  uploadTask.snapshot.ref.getDownloadURL().then(function (downloadURL) {
                    console.log("File available at", downloadURL);
                    console.log(this);
                    _this.downloadURL=downloadURL;
                    _this.show = true;
                  }
                )}  
              )
        },
        
        //データ追加
        post: function(){
            const testId = firebase.firestore().collection('memos').doc().id; //ユニークなIDを生成
            const docRef = firebase.firestore().collection('memos').doc(testId);
            const setAda = docRef.set({
                title: this.title,
                name: this.name,
                memo:this.memo,
                url:this.downloadURL,
            })
            //投稿ボタンを押したらCurry一覧ページに遷移
            this.$router.push('/')
                   
        },
    },
  mounted(){
    //ページ読み込み時に実行される
    this.init();
  },
}
</script>
