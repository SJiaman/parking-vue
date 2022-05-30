<template>
  <div class="mod-demo-echarts">
    <el-row :gutter="30" type="flex" justify="center">
      <el-col :span="6">
        <el-card class="box-card">
          <div>
            <span class="card-panel-num">500</span>
            <div class="line"></div>
            <div>
              <span>总车位数</span>
            </div>
          </div>
        </el-card>
      </el-col>
      <el-col :span="6">
        <el-card class="box-card">
          <div>
            <span class="card-panel-num">480</span>
            <div class="line"></div>
            <div>
              <span>剩余车位数</span>
            </div>
          </div>
        </el-card>
      </el-col>
      <el-col :span="6" v-if="userName == 'admin'">
        <el-card class="box-card">
          <div>
            <span class="card-panel-num">15</span>
            <div class="line"></div>
            <div>
              <span>总订单数</span>
            </div>
          </div>
        </el-card>
      </el-col>
      <el-col :span="6" v-if="userName == 'admin'">
        <el-card class="box-card">
          <div>
            <span class="card-panel-num">320</span>
            <div class="line"></div>
            <div>
              <span>总交易额</span>
            </div>
          </div>
        </el-card>
      </el-col>
    </el-row>
    <el-row :gutter="20" v-if="userName == 'admin'">
      <el-col :span="12">
        <el-card>
          <div id="J_chartLineBox" class="chart-box"></div>
        </el-card>
      </el-col>

      <el-col :span="12">
        <el-card>
          <div id="J_chartBarBox" class="chart-box"></div>
        </el-card>
      </el-col>
    </el-row>

    <el-row v-if="userName != 'admin'" type="flex" justify="center">
      <el-col :span="12">
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
            <el-button type="primary" @click="entryParking()">入库</el-button>
            <el-button type="success" @click="outParking()">出库</el-button>
            <!-- <el-button @click="setImage()">拍照</el-button> -->
          </div>
          <!-- <span>{{parkInfo.name}}</span> -->
        </el-card>
      </el-col>
      <!-- <el-col :span="12"
        ><el-card>
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
          <div class="button">
            <el-button @click="getCompetence()">出库打开</el-button>
            <el-button @click="stopNavigator()">出库关闭</el-button>
            <el-button @click="setImage()">拍照</el-button>
          </div>
          <div v-if="imgSrc" class="img_bg_camera">
            <p>效果预览</p>
            <img :src="imgSrc" alt class="tx_img" />
          </div>
        </el-card>
      </el-col> -->
    </el-row>
  </div>
</template>

<script>
import echarts from "echarts";
import axios from "axios";
export default {
  data() {
    return {
      chartLine: null,
      chartBar: null,
      chartPie: null,
      chartScatter: null,
      videoWidth: 530,
      videoHeight: 350,
      imgSrc: "",
      thisCancas: null,
      thisContext: null,
      thisVideo: null,
      openVideo: false,
      parkInfo: null,
    };
  },
  mounted() {
    if (this.$store.state.user.name != "admin") {
      // 第一步打开摄像头
      this.getCompetence(); //调用摄像头
    } else {
      this.initChartLine();
      this.initChartBar();
    }
  },
  activated() {
    // 由于给echart添加了resize事件, 在组件激活时需要重新resize绘画一次, 否则出现空白bug
    if (this.chartLine) {
      this.chartLine.resize();
    }
    if (this.chartBar) {
      this.chartBar.resize();
    }
    this.parkingInfo();
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
      this.$alert("入库失败", {
        confirmButtonText: "确定",
        callback: (action) => {
          this.$message({
            type: "error",
            message: "入库失败",
          });
        },
      });
    },

    msgAlert(msg) {
      this.$message({
        showClose: true,
        message: msg,
        type: "success",
      });
    },

    parkingInfo() {
      this.dataListLoading = true;
      this.$http({
        url: this.$http.adornUrl("/pms/parkinginfo/info"),
        method: "get",
      }).then(({ data }) => {
        if (data && data.code === 0) {
          console.log(data);
          this.parkInfo = data.parkingInfo;
        } else {
          console.log(data);
        }
      });
    },

    entryParking() {
      var _this = this;
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
            `/pms/access/entry?token=${this.$cookie.get("token")}`
          ),
          formData
        )
        .then((res) => {
          // console.log(res);
          if (res.data.code == "0") {
            // // 图片文件传至后台 == 获取到该图片的url路径
            // this.postVideoImg = res.data.imagePath;
            console.log(res.data);
            _this.msgAlert(res.data.entryMsg);
            var utterThis = new window.SpeechSynthesisUtterance(
              res.data.entryMsg
            );
            utterThis.rate = 0.5;
            window.speechSynthesis.speak(utterThis);

            //获得图片的url后，需要做什么
            //做的事情......
          }
        })
        .catch((error) => {
          this.failAlert();
          //  var utterThis = new window.SpeechSynthesisUtterance(error.msg);
          //   utterThis.rate = 0.5;
          //   window.speechSynthesis.speak(utterThis);
          // console.log(error);
        });
    },

    outParking() {
      var _this = this;
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
            `/pms/access/out?token=${this.$cookie.get("token")}`
          ),
          formData
        )
        .then((res) => {
          // console.log(res);
          if (res.data.code == "0") {
            // 图片文件传至后台 == 获取到该图片的url路径
            // this.postVideoImg = res.data.imagePath;
            console.log(res.data);
            _this.msgAlert(res.data.outMsg);
            if (res.data.accessInfoEntity.carType == 0) {
              this.$http({
                url: this.$http.adornUrl("/pms/ali-pay/trade/page/pay/"),
                method: "post",
                data: this.$http.adornData(res.data.accessInfoEntity, false),
              }).then((response) => {
                //将支付宝返回的表单字符串写在浏览器中，表单会自动触发submit提交
                document.write(response.data.formStr);
              });
            }
            console.log(res.data.outMsg);
            var utterThis = new window.SpeechSynthesisUtterance(
              res.data.outMsg
            );
            utterThis.rate = 0.5;
            window.speechSynthesis.speak(utterThis);
            //获得图片的url后，需要做什么
            //做的事情......
          }
        })
        .catch((error) => {
          console.log(error)
          this.failAlert();
          //  var utterThis = new window.SpeechSynthesisUtterance(error.msg);
          //   utterThis.rate = 0.5;
          //   window.speechSynthesis.speak(utterThis);
          // console.log(error);
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

    // 折线图
    initChartLine() {
      var option = {
        title: {
          text: "近七日进出流量（次）",
        },
        tooltip: {
          trigger: "axis",
        },
        grid: {
          left: "3%",
          right: "4%",
          bottom: "3%",
          containLabel: true,
        },
        toolbox: {
          feature: {
            saveAsImage: {},
          },
        },
        xAxis: {
          type: "category",
          boundaryGap: false,
          data: ["周一", "周二", "周三", "周四", "周五", "周六", "周日"],
        },
        yAxis: {
          type: "value",
        },
        series: [
          {
            name: "邮件营销",
            type: "line",
            stack: "总量",
            data: [20, 32, 11, 34, 20, 30, 10],
          },
        ],
      };
      this.chartLine = echarts.init(
        document.getElementById("J_chartLineBox"),
        "walden"
      );
      this.chartLine.setOption(option);
      window.addEventListener("resize", () => {
        this.chartLine.resize();
      });
    },
    // 柱状图
    initChartBar() {
      var option = {
         title: {
          text: "近七日交易额（元）",
        },
        tooltip: {
          trigger: "axis",
          axisPointer: {
            type: "shadow",
          },
        },
        legend: {
          data: [
            "直接访问",
          ],
        },
        grid: {
          left: "3%",
          right: "4%",
          bottom: "3%",
          containLabel: true,
        },
        xAxis: [
          {
            type: "category",
            data: ["周一", "周二", "周三", "周四", "周五", "周六", "周日"],
          },
        ],
        yAxis: [
          {
            type: "value",
          },
        ],
        series: [
          {
            name: "搜索引擎",
            type: "bar",
            data: [20, 10, 60, 50, 40, 60, 50],
            markLine: {
              lineStyle: {
                normal: {
                  type: "dashed",
                },
              },
              data: [[{ type: "min" }, { type: "max" }]],
            },
          },
        ],
      };
      this.chartBar = echarts.init(document.getElementById("J_chartBarBox"));
      this.chartBar.setOption(option);
      window.addEventListener("resize", () => {
        this.chartBar.resize();
      });
    },
  },
};
</script>

<style lang="scss">
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

.card-panel-num {
  font-size: 28px;
}
.line {
  margin-top: 18px;
  margin-bottom: 20px;
  width: 600px;
  height: 1px;
  // background-color: rgba(235, 238, 245);
  text-align: center;
  font-size: 16px;
  // color: rgba(235, 238, 245);
}
.menu {
  margin-top: 20px;
}
.menu-item {
  margin-left: 24px;
}
.menu-title {
  font-size: 10px;
}

.statistics {
  margin-top: 20px;
}
/* .statistics-title{
  position: relative;
  bottom:  20px;
  left: 30px;
} */

/* .menu-header{
  display: flex;
  justify-items: flex-start;
} */
.chart-box {
  min-height: 400px;
}

.panel-group {
  margin-top: 18px;

  .card-panel-col {
    margin-bottom: 32px;
  }

  .card-panel {
    height: 108px;
    cursor: pointer;
    font-size: 12px;
    position: relative;
    overflow: hidden;
    color: #666;
    background: #fff;
    box-shadow: 4px 4px 40px rgba(0, 0, 0, 0.05);
    border-color: rgba(0, 0, 0, 0.05);

    &:hover {
      .card-panel-icon-wrapper {
        color: #fff;
      }

      .icon-people {
        background: #40c9c6;
      }

      .icon-message {
        background: #36a3f7;
      }

      .icon-money {
        background: #f4516c;
      }

      .icon-shopping {
        background: #34bfa3;
      }
    }

    .icon-people {
      color: #40c9c6;
    }

    .icon-message {
      color: #36a3f7;
    }

    .icon-money {
      color: #f4516c;
    }

    .icon-shopping {
      color: #34bfa3;
    }

    .card-panel-icon-wrapper {
      float: left;
      margin: 14px 0 0 14px;
      padding: 16px;
      transition: all 0.38s ease-out;
      border-radius: 6px;
    }

    .card-panel-icon {
      float: left;
      font-size: 48px;
    }

    .card-panel-description {
      float: right;
      font-weight: bold;
      margin: 26px;
      margin-left: 0px;

      .card-panel-text {
        line-height: 18px;
        color: rgba(0, 0, 0, 0.45);
        font-size: 16px;
        margin-bottom: 12px;
      }

      .card-panel-num {
        font-size: 20px;
      }
    }
  }
}

@media (max-width: 550px) {
  .card-panel-description {
    display: none;
  }

  .card-panel-icon-wrapper {
    float: none !important;
    width: 100%;
    height: 100%;
    margin: 0 !important;

    .svg-icon {
      display: block;
      margin: 14px auto !important;
      float: none !important;
    }
  }
}
</style>
