<template>
  <div class="mod-demo-echarts">
    <!-- <el-alert
      title="提示："
      type="warning"
      :closable="false">
      <div slot-scope="description">
        <p class="el-alert__description">1. 此Demo只提供ECharts官方使用文档，入门部署和体验功能。具体使用请参考：http://echarts.baidu.com/index.html</p>
      </div>
    </el-alert> -->

    <!-- <el-row :gutter="40" class="panel-group">
      <el-col :xs="12" :sm="12" :lg="6" class="card-panel-col">
        <div class="card-panel" @click="handleSetLineChartData('newVisitis')">
          <div class="card-panel-icon-wrapper icon-people">
            <svg-icon icon-class="peoples" class-name="card-panel-icon" />
          </div>
          <div class="card-panel-description">
            <div class="card-panel-text">New Visits</div>
            <count-to
              :start-val="0"
              :end-val="102400"
              :duration="2600"
              class="card-panel-num"
            />
          </div>
        </div>
      </el-col>
      <el-col :xs="12" :sm="12" :lg="6" class="card-panel-col">
        <div class="card-panel" @click="handleSetLineChartData('messages')">
          <div class="card-panel-icon-wrapper icon-message">
            <svg-icon icon-class="message" class-name="card-panel-icon" />
          </div>
          <div class="card-panel-description">
            <div class="card-panel-text">Messages</div>
            <count-to
              :start-val="0"
              :end-val="81212"
              :duration="3000"
              class="card-panel-num"
            />
          </div>
        </div>
      </el-col>
      <el-col :xs="12" :sm="12" :lg="6" class="card-panel-col">
        <div class="card-panel" @click="handleSetLineChartData('purchases')">
          <div class="card-panel-icon-wrapper icon-money">
            <svg-icon icon-class="money" class-name="card-panel-icon" />
          </div>
          <div class="card-panel-description">
            <div class="card-panel-text">Purchases</div>
            <count-to
              :start-val="0"
              :end-val="9280"
              :duration="3200"
              class="card-panel-num"
            />
          </div>
        </div>
      </el-col>
      <el-col :xs="12" :sm="12" :lg="6" class="card-panel-col">
        <div class="card-panel" @click="handleSetLineChartData('shoppings')">
          <div class="card-panel-icon-wrapper icon-shopping">
            <svg-icon icon-class="shopping" class-name="card-panel-icon" />
          </div>
          <div class="card-panel-description">
            <div class="card-panel-text">Shoppings</div>
            <count-to
              :start-val="0"
              :end-val="13600"
              :duration="3600"
              class="card-panel-num"
            />
          </div>
        </div>
      </el-col>
    </el-row> -->

    <el-row :gutter="20" v-if="userName == 'admin'">
      <el-col :span="24">
        <el-card>
          <div id="J_chartLineBox" class="chart-box"></div>
        </el-card>
      </el-col>

      <el-col :span="24">
        <el-card>
          <div id="J_chartBarBox" class="chart-box"></div>
        </el-card>
      </el-col>
      <el-col :span="12">
        <el-card>
          <div id="J_chartPieBox" class="chart-box"></div>
        </el-card>
      </el-col>
      <el-col :span="12">
        <el-card>
          <div id="J_chartScatterBox" class="chart-box"></div>
        </el-card>
      </el-col>
    </el-row>

    <!-- <el-row :gutter="40" class="panel-group">
      <el-col :xs="12" :sm="12" :lg="6" class="card-panel-col">
        <div class="card-panel">
          <div class="card-panel-icon-wrapper icon-people">
            <svg-icon icon-class="peoples" class-name="card-panel-icon" />
          </div>
          <div class="card-panel-description">
            <div class="card-panel-text">New Visits</div>
            <count-to
              :start-val="0"
              :end-val="102400"
              :duration="2600"
              class="card-panel-num"
            />
          </div>
        </div>
      </el-col>
      <el-col :xs="12" :sm="12" :lg="6" class="card-panel-col">
        <div class="card-panel">
          <div class="card-panel-icon-wrapper icon-message">
            <svg-icon icon-class="message" class-name="card-panel-icon" />
          </div>
          <div class="card-panel-description">
            <div class="card-panel-text">Messages</div>
            <count-to
              :start-val="0"
              :end-val="81212"
              :duration="3000"
              class="card-panel-num"
            />
          </div>
        </div>
      </el-col>
      <el-col :xs="12" :sm="12" :lg="6" class="card-panel-col">
        <div class="card-panel">
          <div class="card-panel-icon-wrapper icon-money">
            <svg-icon icon-class="money" class-name="card-panel-icon" />
          </div>
          <div class="card-panel-description">
            <div class="card-panel-text">Purchases</div>
            <count-to
              :start-val="0"
              :end-val="9280"
              :duration="3200"
              class="card-panel-num"
            />
          </div>
        </div>
      </el-col>
      <el-col :xs="12" :sm="12" :lg="6" class="card-panel-col">
        <div class="card-panel">
          <div class="card-panel-icon-wrapper icon-shopping">
            <svg-icon icon-class="shopping" class-name="card-panel-icon" />
          </div>
          <div class="card-panel-description">
            <div class="card-panel-text">Shoppings</div>
            <count-to
              :start-val="0"
              :end-val="13600"
              :duration="3600"
              class="card-panel-num"
            />
          </div>
        </div>
      </el-col>
    </el-row> -->

    <el-row :gutter="50" type="flex" justify="center">
      <el-col :span="6">
        <el-card class="box-card">
          <!-- <div slot="header" class="clearfix">
            <span>会员总数</span>
            <el-tag
              style="float: right"
              type="success"
              size="small"
              plain
              disabled
              >全平台
            </el-tag>
          </div> -->
          <div>
            <span class="card-panel-num">2</span>
            <div class="line"></div>
            <div>
              <span>今日订单数</span>
              <span style="float: right">q单</span>
            </div>
          </div>
        </el-card>
      </el-col>
      <el-col :span="6">
        <el-card class="box-card">
          <!-- <div slot="header" class="clearfix"> -->
          <!-- <span>会员总数</span>
            <el-tag
              style="float: right"
              type="success"
              size="small"
              plain
              disabled
              >全平台
            </el-tag>
          </div> -->
          <div>
            <span class="card-panel-num">2</span>
            <div class="line"></div>
            <div>
              <span>今日订单数</span>
              <span style="float: right">q单</span>
            </div>
          </div>
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
            <el-button type="primary" @click="entryParking()"
              >入库</el-button
            >
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
// import walden from '@/assets/js/walden'
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
      recognitionMsg: "贵AD7893,会员车",
      parkInfo: null,
    };
  },
  mounted() {
    // this.initChartLine();
    // this.initChartBar();
    // this.initChartPie();
    // this.initChartScatter();
    if (this.$store.state.user.name != "admin") {
      // 第一步打开摄像头
      this.getCompetence(); //调用摄像头
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
    if (this.chartPie) {
      this.chartPie.resize();
    }
    if (this.chartScatter) {
      this.chartScatter.resize();
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
            _this.successAlert(res.data.entryMsg);
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
            _this.successAlert(res.data.outMsg);
            if (res.data.carType == "0") {
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
            var utterThis = new window.SpeechSynthesisUtterance(res.data.outMsg);
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
          text: "折线图堆叠",
        },
        tooltip: {
          trigger: "axis",
        },
        legend: {
          data: ["邮件营销", "联盟广告", "视频广告", "直接访问", "搜索引擎"],
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
            data: [120, 132, 101, 134, 90, 230, 210],
          },
          {
            name: "联盟广告",
            type: "line",
            stack: "总量",
            data: [220, 182, 191, 234, 290, 330, 310],
          },
          {
            name: "视频广告",
            type: "line",
            stack: "总量",
            data: [150, 232, 201, 154, 190, 330, 410],
          },
          {
            name: "直接访问",
            type: "line",
            stack: "总量",
            data: [320, 332, 301, 334, 390, 330, 320],
          },
          {
            name: "搜索引擎",
            type: "line",
            stack: "总量",
            data: [820, 932, 901, 934, 1290, 1330, 1320],
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
        tooltip: {
          trigger: "axis",
          axisPointer: {
            type: "shadow",
          },
        },
        legend: {
          data: [
            "直接访问",
            "邮件营销",
            "联盟广告",
            "视频广告",
            "搜索引擎",
            "百度",
            "谷歌",
            "必应",
            "其他",
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
            name: "直接访问",
            type: "bar",
            data: [320, 332, 301, 334, 390, 330, 320],
          },
          {
            name: "邮件营销",
            type: "bar",
            stack: "广告",
            data: [120, 132, 101, 134, 90, 230, 210],
          },
          {
            name: "联盟广告",
            type: "bar",
            stack: "广告",
            data: [220, 182, 191, 234, 290, 330, 310],
          },
          {
            name: "视频广告",
            type: "bar",
            stack: "广告",
            data: [150, 232, 201, 154, 190, 330, 410],
          },
          {
            name: "搜索引擎",
            type: "bar",
            data: [862, 1018, 964, 1026, 1679, 1600, 1570],
            markLine: {
              lineStyle: {
                normal: {
                  type: "dashed",
                },
              },
              data: [[{ type: "min" }, { type: "max" }]],
            },
          },
          {
            name: "百度",
            type: "bar",
            barWidth: 5,
            stack: "搜索引擎",
            data: [620, 732, 701, 734, 1090, 1130, 1120],
          },
          {
            name: "谷歌",
            type: "bar",
            stack: "搜索引擎",
            data: [120, 132, 101, 134, 290, 230, 220],
          },
          {
            name: "必应",
            type: "bar",
            stack: "搜索引擎",
            data: [60, 72, 71, 74, 190, 130, 110],
          },
          {
            name: "其他",
            type: "bar",
            stack: "搜索引擎",
            data: [62, 82, 91, 84, 109, 110, 120],
          },
        ],
      };
      this.chartBar = echarts.init(document.getElementById("J_chartBarBox"));
      this.chartBar.setOption(option);
      window.addEventListener("resize", () => {
        this.chartBar.resize();
      });
    },
    // 饼状图
    initChartPie() {
      var option = {
        backgroundColor: "#2c343c",
        title: {
          text: "Customized Pie",
          left: "center",
          top: 20,
          textStyle: {
            color: "#ccc",
          },
        },
        tooltip: {
          trigger: "item",
          formatter: "{a} <br/>{b} : {c} ({d}%)",
        },
        visualMap: {
          show: false,
          min: 80,
          max: 600,
          inRange: {
            colorLightness: [0, 1],
          },
        },
        series: [
          {
            name: "访问来源",
            type: "pie",
            radius: "55%",
            center: ["50%", "50%"],
            data: [
              { value: 335, name: "直接访问" },
              { value: 310, name: "邮件营销" },
              { value: 274, name: "联盟广告" },
              { value: 235, name: "视频广告" },
              { value: 400, name: "搜索引擎" },
            ].sort(function (a, b) {
              return a.value - b.value;
            }),
            roseType: "radius",
            label: {
              normal: {
                textStyle: {
                  color: "rgba(255, 255, 255, 0.3)",
                },
              },
            },
            labelLine: {
              normal: {
                lineStyle: {
                  color: "rgba(255, 255, 255, 0.3)",
                },
                smooth: 0.2,
                length: 10,
                length2: 20,
              },
            },
            itemStyle: {
              normal: {
                color: "#c23531",
                shadowBlur: 200,
                shadowColor: "rgba(0, 0, 0, 0.5)",
              },
            },
            animationType: "scale",
            animationEasing: "elasticOut",
            animationDelay: function (idx) {
              return Math.random() * 200;
            },
          },
        ],
      };
      this.chartPie = echarts.init(document.getElementById("J_chartPieBox"));
      this.chartPie.setOption(option);
      window.addEventListener("resize", () => {
        this.chartPie.resize();
      });
    },
    // 散点图
    initChartScatter() {
      var option = {
        backgroundColor: new echarts.graphic.RadialGradient(0.3, 0.3, 0.8, [
          { offset: 0, color: "#f7f8fa" },
          { offset: 1, color: "#cdd0d5" },
        ]),
        title: {
          text: "1990 与 2015 年各国家人均寿命与 GDP",
        },
        legend: {
          right: 10,
          data: ["1990", "2015"],
        },
        xAxis: {
          splitLine: {
            lineStyle: {
              type: "dashed",
            },
          },
        },
        yAxis: {
          splitLine: {
            lineStyle: {
              type: "dashed",
            },
          },
          scale: true,
        },
        series: [
          {
            name: "1990",
            data: [
              [28604, 77, 17096869, "Australia", 1990],
              [31163, 77.4, 27662440, "Canada", 1990],
              [1516, 68, 1154605773, "China", 1990],
              [13670, 74.7, 10582082, "Cuba", 1990],
              [28599, 75, 4986705, "Finland", 1990],
              [29476, 77.1, 56943299, "France", 1990],
              [31476, 75.4, 78958237, "Germany", 1990],
              [28666, 78.1, 254830, "Iceland", 1990],
              [1777, 57.7, 870601776, "India", 1990],
              [29550, 79.1, 122249285, "Japan", 1990],
              [2076, 67.9, 20194354, "North Korea", 1990],
              [12087, 72, 42972254, "South Korea", 1990],
              [24021, 75.4, 3397534, "New Zealand", 1990],
              [43296, 76.8, 4240375, "Norway", 1990],
              [10088, 70.8, 38195258, "Poland", 1990],
              [19349, 69.6, 147568552, "Russia", 1990],
              [10670, 67.3, 53994605, "Turkey", 1990],
              [26424, 75.7, 57110117, "United Kingdom", 1990],
              [37062, 75.4, 252847810, "United States", 1990],
            ],
            type: "scatter",
            symbolSize: function (data) {
              return Math.sqrt(data[2]) / 5e2;
            },
            label: {
              emphasis: {
                show: true,
                formatter: function (param) {
                  return param.data[3];
                },
                position: "top",
              },
            },
            itemStyle: {
              normal: {
                shadowBlur: 10,
                shadowColor: "rgba(120, 36, 50, 0.5)",
                shadowOffsetY: 5,
                color: new echarts.graphic.RadialGradient(0.4, 0.3, 1, [
                  { offset: 0, color: "rgb(251, 118, 123)" },
                  { offset: 1, color: "rgb(204, 46, 72)" },
                ]),
              },
            },
          },
          {
            name: "2015",
            data: [
              [44056, 81.8, 23968973, "Australia", 2015],
              [43294, 81.7, 35939927, "Canada", 2015],
              [13334, 76.9, 1376048943, "China", 2015],
              [21291, 78.5, 11389562, "Cuba", 2015],
              [38923, 80.8, 5503457, "Finland", 2015],
              [37599, 81.9, 64395345, "France", 2015],
              [44053, 81.1, 80688545, "Germany", 2015],
              [42182, 82.8, 329425, "Iceland", 2015],
              [5903, 66.8, 1311050527, "India", 2015],
              [36162, 83.5, 126573481, "Japan", 2015],
              [1390, 71.4, 25155317, "North Korea", 2015],
              [34644, 80.7, 50293439, "South Korea", 2015],
              [34186, 80.6, 4528526, "New Zealand", 2015],
              [64304, 81.6, 5210967, "Norway", 2015],
              [24787, 77.3, 38611794, "Poland", 2015],
              [23038, 73.13, 143456918, "Russia", 2015],
              [19360, 76.5, 78665830, "Turkey", 2015],
              [38225, 81.4, 64715810, "United Kingdom", 2015],
              [53354, 79.1, 321773631, "United States", 2015],
            ],
            type: "scatter",
            symbolSize: function (data) {
              return Math.sqrt(data[2]) / 5e2;
            },
            label: {
              emphasis: {
                show: true,
                formatter: function (param) {
                  return param.data[3];
                },
                position: "top",
              },
            },
            itemStyle: {
              normal: {
                shadowBlur: 10,
                shadowColor: "rgba(25, 100, 150, 0.5)",
                shadowOffsetY: 5,
                color: new echarts.graphic.RadialGradient(0.4, 0.3, 1, [
                  { offset: 0, color: "rgb(129, 227, 238)" },
                  { offset: 1, color: "rgb(25, 183, 207)" },
                ]),
              },
            },
          },
        ],
      };
      this.chartPie = echarts.init(
        document.getElementById("J_chartScatterBox")
      );
      this.chartPie.setOption(option);
      window.addEventListener("resize", () => {
        this.chartPie.resize();
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
