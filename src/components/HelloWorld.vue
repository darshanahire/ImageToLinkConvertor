<template>
  <div v-if="!loader" class="hello">
    <h1>{{ msg }}</h1>
    <div style="margin:30px auto">
    <input id="input" class="input" type="file" accept="image/*" @change="openFile" hidden >
    <label class="cameraParent" for="input" title="Select to choose Photo" style="cursor: pointer">
      Click <span style="color:blue"> here </span> Select Image
    </label>
    </div>
    <button @click="upload"> Upload Image</button>
   <div class="p">
      <p v-if="url">
        <span style="font-weight : bold">Copy This Url :</span>
    {{url}}
    </p>
   </div>
  </div>
<div v-else>
  <img src="@/assets/best loader.gif" alt="">
  <p>Your Link is Getting Ready... </p>
    <p>Please wait...</p>
</div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data(){
    return {
      url:'',
      loader:false
    }
  },
   methods: {
    openFile(file) {      
       var input = file.target;

      var reader = new FileReader();
      reader.readAsDataURL(input.files[0]);
        this.image=input.files[0]
    },
      upload(){
        this.loader = true;
      if(this.image!=null){
      const formdata = new FormData();
        formdata.append("file",this.image );
        formdata.append("upload_preset", "b1mhgyub")

        axios.post("https://api.cloudinary.com/v1_1/darshanscloud/image/upload", formdata).then(async (res) => {
          this.loader = false;
          this.url = res.data.secure_url;
        })}
        else{
          alert("something went wrong");
        }

      
    },
      AddBorderBottom(e) {
      e.target.style.borderBottom = "2px solid #00bfa5";
    },
  },
}
</script>

<style scoped>

.input{
  text-align: center;
  width: 70%;
  height: 40px;
  font-size: 20px;
}
button{
  height: 30px;
  width: 50%;
  margin: 30px;
}
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
