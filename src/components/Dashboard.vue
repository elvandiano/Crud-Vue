<template>
  <div id="dash" class="container">
    <div class="page-header wow animated bounceInDown">
      <div class="lg" id="lgn">
        <input placeholder="username" id="username">
        <input placeholder="password" id="password">
        <button id="btn-login" @click="login()">Login</button>
      </div>
      <div class="lg" id="logout" hidden>
        <button id="btn-logut" @click="logout()">Logout</button>
      </div>
      <h1>Cari Jodoh by Facebook</h1>
    </div>
    <div class="panel panel-primary" id="form-tambah" hidden>
      <div class="panel-heading">
        <h3 style="text-align: center">Tambah Peserta</h3>
      </div>
      <div class="panel-body">
        <div id="form" class="form-group" @submit.prevent="tambahPeserta">
          <div class="col-md-3">
            <label for="nama">Nama:</label>
            <input id="nama" placeholder="Masukkan Nama..." class="form-control" v-model="newGirl.nama">
          </div>
          <div class="col-md-3">
            <label for="status">Status:</label>
            <input id="status" placeholder="Masukkan Status..." class="form-control" v-model="newGirl.status">
          </div>
          <div class="col-md-3">
            <label for="tgl-lahir">Tanggal Lahir:</label>
            <input id="tgl-lahir" placeholder="Masukkan Tgl. Lahir..." class="form-control" v-model="newGirl.tgl_lahir">
          </div>
          <div class="col-md-3">
            <label for="url-fb">Facebook:</label>
            <input id="url-fb" placeholder="Masukkan Alamat FB..." class="form-control" v-model="newGirl.url_facebook">
          </div>
        </div>
        <div class="form-group">
          <div class="col-md-6" align="left">
            <input class="btn btn-info" type="submit" style="margin-top: 10px; font-weight: bold" value="Simpan" @click="tambahPeserta()">
            <input class="btn btn-default" type="submit" style="margin-top: 10px; font-weight: bold; background-color: gainsboro" value="Keluar" @click="keluar()">
          </div>
        </div>
      </div>
    </div>
    <div class="form-group" id="tbh" hidden>
      <input class="btn btn-primary wow animated bounceInLeft" type="submit" style="margin-top: 10px; font-weight: bold" value="Tambah Peserta" @click="tampil()">
    </div>
    <div class="panel-group wow animated bounceInUp" id="tbl" hidden >
      <div class="panel panel-primary">
        <div class="panel-heading" >
          <h3 style="text-align: center">Daftar Orang Lajang di IconPlus</h3>
        </div>
        <div class="panel-body">
          <div class="from-group">
            <div class="col-md-12">
              <table class="table table-striped">
                <thead>
                <tr>
                  <th>Nama</th>
                  <th>Status</th>
                  <th>Tgl. Lahir</th>
                  <th>Facebook</th>
                  <th>Aksi</th>
                </tr>
                </thead>
                <tbody v-for="girl in girls">
                <tr v-if="!girl.edit">
                  <td>{{girl.nama}}</td>
                  <td>{{girl.status}}</td>
                  <td>{{girl.tgl_lahir}}</td>
                  <td>
                    <a v-bind:href="girl.url_facebook" class="fa fa-facebook-square"></a>
                  </td>
                  <td>
                    <a href="#" style="margin-right: 8px" @click="remove(girl)"><span class="glyphicon glyphicon-trash"></span></a>
                    <a href="#" style="margin-left: 8px" @click="setedit(girl)"><span class="glyphicon glyphicon-edit"></span></a>
                  </td>
                </tr>
                <tr v-else>
                  <td> <input v-model="newGirl.editNama"></td>
                  <td> <input v-model="newGirl.editStatus"></td>
                  <td> <input v-model="newGirl.editTgl_lahir"></td>
                  <td> <input v-model="newGirl.editUrl_facebook"></td>
                  <td>
                    <a href="#" style="margin-right: 8px" @click="batal(girl)"><span class="glyphicon glyphicon-share"></span></a>
                    <a href="#" style="margin-left: 8px" @click="savedata(girl)"><span class="glyphicon glyphicon-floppy-saved"></span></a>
                  </td>
                </tr>
                </tbody>
              </table>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
    // u must import what u need //
    ///////////////////////////////

    // importing vuesax
    // import Vuesax from 'vuesax'

    // importing toastr
    import toastr from 'toastr'

    // importing firebase
    import Firebase from 'firebase'


    //this config from database of firebase to make a relation to the database
    let config = {
      apiKey: "AIzaSyCpXMzmVbSOD5MYGcCPeqgZeF30kdPTO38",
      authDomain: "vuejs-firebase-first-10884.firebaseapp.com",
      databaseURL: "https://vuejs-firebase-first-10884.firebaseio.com",
      projectId: "vuejs-firebase-first-10884",
      storageBucket: "vuejs-firebase-first-10884.appspot.com",
      messagingSenderId: "982149792464"
    }

    //initialize config from firebase
    let app = Firebase.initializeApp(config);

    //create a variabel of database
    let db = app.database();

    //crate a variable of database reference
    let girlRef = db.ref('girls');

    export default {
      name: 'dashboard',
      firebase: {
        girls: girlRef
      },
      data () {
        return {
          newGirl : {
            nama : '',
            status : '',
            tgl_lahir : '',
            url_facebook : ''
          }
        }
      },
      created(){
        console.log(this.girls);
      },
      methods: {

        //SET: TIDAK DI ANJURKAN UNTUK DIGUNAKAN >> digunakan untuk reWrite || replace, jadi kalau dia nambah bikin baru dan menghapus data yang udah ada cek >> https://www.youtube.com/watch?v=wcUSiAmvcRQ&index=4&list=PLCZlgfAG0GXCMaYN4ZYiuyHRG5KSGd2la

        //PUSH : Add dan generate Auto id.. digunakan untuk menambah data dan Id nya sudah tergenerate Automatis, cek >> https://www.youtube.com/watch?v=wcUSiAmvcRQ&index=4&list=PLCZlgfAG0GXCMaYN4ZYiuyHRG5KSGd2la

        //UPDATE : dia mengUpdate berdasarkan ID, dia juga bisa nambahin jika Id nya blm tersedia, dan mengEdit jika id nya sudah tersedia

        //REMOVE : untuk menghapus data berdasarkan KEY

        tambahPeserta: function () {
          //set form add
          if ( this.newGirl.nama !== "" && this.newGirl.status !== "" && this.newGirl.tgl_lahir !== "" && this.newGirl.url_facebook !== "") {
            girlRef.push({
              edit: false,
              nama : this.newGirl.nama,
              status : this.newGirl.status,
              tgl_lahir: this.newGirl.tgl_lahir,
              url_facebook: this.newGirl.url_facebook
            });

            this.newGirl.nama = '';
            this.newGirl.status= '';
            this.newGirl.tgl_lahir= '';
            this.newGirl.url_facebook= '';
          }else {
            alert("lengkapi data terlebih dahulu");
          }

        },
        // remove's function
        remove: function (girl) {
          if (confirm("Apa kamu yakin?")) {
            let del = girlRef.child(girl['.key']);
            del.remove();

            //using toastr success for notification when u success deleting data
            toastr.success("Data berhasil dihapus");
          }

        },
        //setedit's function
        setedit: function (girl) {
          let upData = girlRef.child(girl['.key']);
          upData.update({edit: true});
          this.newGirl.editNama = girl.nama;
          this.newGirl.editStatus = girl.status;
          this.newGirl.editTgl_lahir = girl.tgl_lahir;
          this.newGirl.editUrl_facebook = girl.url_facebook;

        },
        //savedata's function
        savedata: function (nama) {
          let sv = girlRef.child(nama['.key']);
          sv.set({
            edit: false,
            nama : this.newGirl.editNama,
            status: this.newGirl.editStatus,
            tgl_lahir: this.newGirl.editTgl_lahir,
            url_facebook: this.newGirl.editUrl_facebook
          });

        },
        //batal's function
        batal: function (girl) {
          let upData = girlRef.child(girl['.key']);
          upData.update({edit: false});

        },
        //tampil's function
        tampil: function () {
          document.getElementById("form-tambah").style.display = "block";

        },
        //keluar's function
        keluar: function () {
          document.getElementById("form-tambah").style.display = "none";

        },
        // login's function
        login: function () {

          let username = document.getElementById("username").value;
          let password = document.getElementById("password").value;

          if( username === "" && password === "") {
            alert("data tidak lengkap")
          } else {
            document.getElementById("tbh").style.display = "block";
            document.getElementById("tbl").style.display = "block";
            document.getElementById("lgn").style.display = "none";
            document.getElementById("logout").style.display = "block";
          }

        },
        // logout's function
        logout : function () {
          document.getElementById("tbh").style.display = "none";
          document.getElementById("tbl").style.display = "none";
          document.getElementById("lgn").style.display = "block";
          document.getElementById("logout").style.display = "none";
        },
      }
    }
</script>

<style scoped>
  /*#dash {*/
    /*font-family: 'Xingkai TC', Helvetica, Arial, sans-serif;*/
    /*-webkit-font-smoothing: antialiased;*/
    /*-moz-osx-font-smoothing: grayscale;*/
    /*color: #2c3e50;*/
    /*margin-top: 60px;*/
    /*font-weight: bold;*/


  /*}*/
  h1 {
    border-radius: 20px;
    padding-bottom: 5px;
    background-color: #4479b2;
    font-weight: bold;
    font-family: "Xingkai TC";
    text-align: center;
    color: azure;

  }
  h3 {
    font-weight: bold;
    font-family: "Xingkai TC";

  }
  table{
    font-weight: bold;

  }
  input{
    font-weight: bold;

  }
  th{
    font-size: 20px;

  }
  label{
    font-size: 20px;

  }

</style>
