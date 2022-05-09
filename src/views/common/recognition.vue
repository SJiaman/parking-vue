<template>
  <div>
    <el-row>
      <el-col :span="12" :push="5">
        <el-card>
          <video
            id="videoCamera"
            :width="videoWidth"
            :height="videoHeight"
            autoplay
          ></video>
          <canvas
            style="display: none"
            id="canvasCamera"
            :width="videoWidth"
            :height="videoHeight"
          ></canvas>
          <br />
          <div style="padding-left: 200px">
            <el-button type="primary" @click="licenseplateRecognition()"
              >入库</el-button
            >
            <el-button type="success" @click="stopNavigator()">出库</el-button>
            <!-- <el-button @click="setImage()">拍照</el-button> -->
          </div>
        </el-card>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      videoWidth: 520,
      videoHeight: 350,
      imgSrc: "",
      thisCancas: null,
      thisContext: null,
      thisVideo: null,
      openVideo: false,
      recognitionMsg: "贵AD7893,会员车",
    };
  },
  mounted() {
    if (this.$store.state.user.name != "admin") {
      // 第一步打开摄像头
      this.getCompetence(); //调用摄像头
    }
  },
  computed: {
    userName: {
      get() {
        return this.$store.state.user.name;
      },
    },
  },
  methods: {
    failAlert() {
      this.$alert(recognitionMsg, "入库失败", {
        confirmButtonText: "确定",
        callback: (action) => {
          this.$message({
            type: "error",
            message: "入库失败",
          });
        },
      });
    },

    successAlert() {
      this.$message({
        showClose: true,
        message: `欢迎${this.recognitionMsg}入库`,
        type: "success",
      });
    },

    licenseplateRecognition() {
      // 生成图表
      this.setImage();
      // 生成文件表单上传
      let formData = new FormData();
      formData.append(
        "file",
        this.dataURLtoFile(this.imgSrc, "licenseplate.png")
      );

      // 使用封装的axios是统一的header，不能上传文件类的内容，所以在这直接使用axios原生的
      // url需要使用本项目的加token和项目前缀
      axios
        .post(
          this.$http.adornUrl(
            `/car/manage/recognition?token=${this.$cookie.get("token")}`
          ),
          formData
        )
        .then((res) => {
          // console.log(res);
          if (res.data.code == "0") {
            // 图片文件传至后台 == 获取到该图片的url路径
            this.postVideoImg = res.data.imagePath;
            this.successAlert();

            //获得图片的url后，需要做什么
            //做的事情......
          }
        })
        .catch((error) => {
          this.failAlert();
          console.log(error);
        });
    },

    getCompetence() {
      var _this = this;
      _this.thisCancas = document.getElementById("canvasCamera");
      _this.thisContext = this.thisCancas.getContext("2d");
      _this.thisVideo = document.getElementById("videoCamera");
      _this.thisVideo.style.display = "block";
      // 获取媒体属性，旧版本浏览器可能不支持mediaDevices，我们首先设置一个空对象
      if (navigator.mediaDevices === undefined) {
        navigator.mediaDevices = {};
      }
      // 一些浏览器实现了部分mediaDevices，我们不能只分配一个对象
      // 使用getUserMedia，因为它会覆盖现有的属性。
      // 这里，如果缺少getUserMedia属性，就添加它。
      if (navigator.mediaDevices.getUserMedia === undefined) {
        navigator.mediaDevices.getUserMedia = function (constraints) {
          // 首先获取现存的getUserMedia(如果存在)
          var getUserMedia =
            navigator.webkitGetUserMedia ||
            navigator.mozGetUserMedia ||
            navigator.getUserMedia;
          // 有些浏览器不支持，会返回错误信息
          // 保持接口一致
          if (!getUserMedia) {
            //不存在则报错
            return Promise.reject(
              new Error("getUserMedia is not implemented in this browser")
            );
          }
          // 否则，使用Promise将调用包装到旧的navigator.getUserMedia
          return new Promise(function (resolve, reject) {
            getUserMedia.call(navigator, constraints, resolve, reject);
          });
        };
      }
      var constraints = {
        audio: false,
        video: {
          width: this.videoWidth,
          height: this.videoHeight,
          transform: "scaleX(-1)",
        },
      };
      navigator.mediaDevices
        .getUserMedia(constraints)
        .then(function (stream) {
          // 旧的浏览器可能没有srcObject
          if ("srcObject" in _this.thisVideo) {
            _this.thisVideo.srcObject = stream;
          } else {
            // 避免在新的浏览器中使用它，因为它正在被弃用。
            _this.thisVideo.src = window.URL.createObjectURL(stream);
          }
          _this.thisVideo.onloadedmetadata = function (e) {
            _this.thisVideo.play();
          };
        })
        .catch((err) => {
          console.log(err);
        });
    },
    //  绘制图片（拍照功能）
    setImage() {
      var _this = this;
      // canvas画图
      _this.thisContext.drawImage(
        _this.thisVideo,
        0,
        0,
        _this.videoWidth,
        _this.videoHeight
      );
      // 获取图片base64链接
      var image = this.thisCancas.toDataURL("image/png");
      _this.imgSrc = image; //赋值并预览图片
    },
    // 关闭摄像头
    stopNavigator() {
      this.thisVideo.srcObject.getTracks()[0].stop();
    },
    // base64转文件，此处没用到
    dataURLtoFile(dataurl, filename) {
      var arr = dataurl.split(",");
      var mime = arr[0].match(/:(.*?);/)[1];
      var bstr = atob(arr[1]);
      var n = bstr.length;
      var u8arr = new Uint8Array(n);
      while (n--) {
        u8arr[n] = bstr.charCodeAt(n);
      }
      return new File([u8arr], filename, { type: mime });
    },
  },
};
</script>

<style lang="scss" scope>
.mod-demo-echarts {
  > .el-alert {
    margin-bottom: 10px;
  }
  > .el-row {
    margin-top: -10px;
    margin-bottom: -10px;
    .el-col {
      padding-top: 10px;
      padding-bottom: 10px;
    }
  }
  .chart-box {
    min-height: 400px;
  }
}
</style>
