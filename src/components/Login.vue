<template>
  <div class="container">
    <div class="background"></div>
    <div class="content">
      <div class="word">毕业<br />都去哪儿了</div>
      <input
        type="file"
        class="un"
        ref="input"
        accept="image/*"
        @change="updateImg"
      />
      <button class="photo" @click="getImage" ref="btn">
        <img src="@/images/camera.png" alt="" v="" ref="img" />
      </button>
      点击上传头像
      <div class="input">
        <input
          type="text"
          placeholder="填写昵称(最多10个字)"
          v-model="userName"
          maxlength="10"
        />
        <div class="warn" v-if="used">
          这个昵称已经存在咯，如果是第一次登录请换一个昵称吧
        </div>
      </div>
      <button class="check" @click="goNext">写好了</button>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      used: 0, //判断昵称是否已被使用,0未使用，1已使用
      next: "loading",
      userName: "", //记录的昵称
      imgUrl: "@/images/camera.png", //图片的地址
      fileTypes: [
        //判断是否为图片类型
        "image/jpg",
        "image/gif",
        "image/webp",
        "image/png",
        "image/jpeg",
      ],
    };
  },
  methods: {
    validFileType(file) {
      return this.fileTypes.includes(file.type);
    },
    goNext() {
      if (this.userName == "") {
        alert("你还没有输入昵称哦！");
        return;
      }
      if (this.imgUrl == "@/images/camera.png") {
        alert("你还没有上传头像哦！");
        return;
      }
      this.$emit("Login", this.imgUrl, this.userName);
      this.$emit("pageChange", this.next);
    },
    getImage() {
      this.$refs.input.click();
    },
    //图片压缩并记录
    imgCompress(image, quality, type) {
      const vue = this;
      var canvas = document.createElement("canvas");
      var img = document.createElement("img");
      canvas.width = 50;
      canvas.height = 50;
      var res;
      img.src = image;
      img.onload = () => {
        let cx = canvas.getContext("2d");
        cx.drawImage(img, 0, 0, 50, 50);
        res = canvas.toDataURL(type, quality / 100);
        console.log(res);
        vue.imgUrl = res;
        vue.$refs.img.src = vue.imgUrl;
        vue.$refs.img.style.width = "22vw";
        vue.$refs.img.style.height = "9vh";
      };
    },
    updateImg() {
      const curFiles = this.$refs.input.files;
      // console.log(curFiles);
      const vue = this;
      var reader = new FileReader();
      for (const file of curFiles) {
        // console.log(file);
        if (this.validFileType(file)) {
          reader.onload = () => {
            if (file.size / 1024 > 1)
              vue.imgCompress(reader.result, 90, "image/png");
            else {
              vue.imgUrl = reader.result;
              vue.$refs.img.src = reader.result;
              vue.$refs.img.style.width = "22vw";
              vue.$refs.img.style.height = "9vh";
            }
          };
          reader.readAsDataURL(file);
        } else alert("选择的不是图片哦，请重试！");
      }
    },
  },
  watch: {
    userName(newVal) {
      if (newVal == "") {
        this.used = 0;
        return;
      }
      let vue = this;
      //发起get请求，判断是否用户名在数据库中有记录
      var config = {
        method: "get",
        params: {
          username: newVal,
        },
        url: "/api/user",
      };

      axios(config)
        .then(function (response) {
          vue.used = 1;
          console.log(JSON.stringify(response.data));
        })
        .catch(function (error) {
          vue.used = 0;
          console.log(error);
        });
    },
  },
};
</script>

<style lang="less" scoped>
.container {
  display: flex;
  background-image: url("@/images/up.png");
  background-repeat: no-repeat;
  background-size: 100%;
  flex-direction: column;
  height: 100vh;
  position: relative;
}

.background {
  height: 30%;
  background-size: cover;
  background-repeat: no-repeat;
  background-position: center top;
}

.content {
  display: flex;
  flex-direction: column;
  flex-wrap: nowrap;
  padding: 0 10%;
  height: 70%;
  background-color: white;
  position: relative;
  border-radius: 5% 5% 0 0;
  font-size: 1.2rem;
  .un {
    opacity: 0;
  }

  .word {
    margin-top: -50%;
    margin-left: -5%;
    font-size: 3.5rem;
    text-align: left;
    color: white;
  }
  .photo {
    width: 25vw;
    height: 25vw;
    border: none;
    margin: 2vw auto;
    margin-top: 15vw;
    border-radius: 7vw;
    img {
      width: 15vw;
    }
  }
  .input {
    margin-top: 20vw;
    background-color: rgba(254, 247, 239, 1);
    width: 80vw;
    height: 13vw;
    border-radius: 5vw;
    input {
      width: 90%;
      height: 100%;
      border: none;
      outline: none;
      background-color: transparent;
    }
    .warn {
      color: red;
      font-size: 0.1rem;
    }
  }
  .check {
    margin-top: 10vw;
    width: 80vw;
    height: 13vw;
    border: none;
    border-radius: 5vw;
    background-color: rgba(44, 113, 134, 1);
    color: white;
  }
}
</style>