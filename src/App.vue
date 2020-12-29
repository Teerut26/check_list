<template>
  <div class="container mt-3 text-block " id="app">
    <div class="card text-left">
      <div class="card-body">
        <h4 class="card-title">สร้างรูปคนที่ไม่มาโรงเรียน</h4>
        <hr>
        <div class="row">
          <div class="col-sm">
            <div class="form-group">
              <label for="">ชื่อคนที่ไม่มาโรงเรียน</label>
              <select v-model="form.name" class="custom-select" >
                <option @click="setNumber(index)" v-for="(item ,index) in nameList" :value="item" :key="index">{{index+1}}.{{item}}</option>
              </select>
            </div>
          </div>
          <div class="col-sm">
            <div class="form-group">
              <label for="">เลขที่คนที่ไม่มาโรงเรียน</label>
              <input v-model="form.no" type="number" class="form-control" placeholder="เลขที่คนที่ลา">
            </div>
          </div>
        </div>
        <div class="row">
          <div class="col-sm">
            <div class="form-group">
              <label for="">สาเหตุที่ไม่มาโรงเรียน</label>
              <select  v-model="form.info" class="custom-select">
                <option value="ลากิจ">ลากิจ</option>
                <option value="ลาป่วย">ลาป่วย</option>
                <option value="ขาด">ขาด</option>
                <option value="อื่นๆ">อื่นๆ</option>
              </select>
            </div>
          </div>
        </div>
        <div class="form-group">
          <button type="button" @click="addPo()" class="btn btn-success mr-1">เพิ่ม</button>
          <button type="button" @click="screenCap()" class="btn btn-primary  mr-1">บันทึกรูปภาพ</button>
          <button type="button" @click="dataObj=[]" class="btn btn-danger"><i class="fas fa-redo-alt"></i></button>
        </div>
        <!-- <div class="form-group">
          <button type="button" @click="downloadJson(dataObj)" class="btn btn-info mr-1"><i class="fas fa-download"></i></button>
          <button type="button" @click="downloadJson(dataObj)" class="btn btn-info"><i class="fas fa-upload"></i></button>
        </div> -->
      </div>
    </div>
    <div id="text" class="text-block mt-3 paper">
      <center>
        <h4 class="card-title text-block">
          <p><i class="fad fa-user-secret"></i> ชั้นมัธยมศึกษาปีที่ 5/3 <i class="fad fa-user-secret"></i></p>
        </h4>
        <h5 class="text-block">
          <p><i class="fad text-danger fa-times"></i> {{dateNow}} <i class="fad text-danger fa-times"></i></p>
        </h5>
        <h5 class="text-block">
          <p><i class="fad text-danger fa-times"></i> {{timeNow}} <i class="fad text-danger fa-times"></i></p>
        </h5>
      </center>
      <div class="table-responsive">
        <table class="table table-bordered" v-if="showTable">
          <tbody>
            <tr v-for="(item, index) in dataObj" :key="item">
              <td>
                <p>{{index+1}}. เลขที่ {{item.no}} {{item.name}}</p>
              </td>
              <td v-if="item.info == 'ลากิจ'"><span class="badge badge-warning">{{item.info}}</span></td>
              <td v-else-if="item.info == 'ลาป่วย'"><span class="badge badge-warning">{{item.info}}</span></td>
              <td v-else-if="item.info == 'ขาด'"><span class="badge badge-danger">{{item.info}}</span></td>
              <td v-else><span class="badge badge-secondary">{{item.info}}</span></td>
            </tr>
          </tbody>
        </table>
        <table class="table table-striped" v-else>
          <thead class="table-dark">
            <tr>
              <th>ข้อมูล</th>
              <th>สาเหตุการลา</th>
              <th>Action</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(item, index) in dataObj" :key="item">
              <td>{{index+1}}. เลขที่ {{item.no}} {{item.name}}</td>
              <td v-if="item.info == 'ลากิจ'"><span class="badge badge-warning">{{item.info}}</span></td>
              <td v-else-if="item.info == 'ลาป่วย'"><span class="badge badge-warning">{{item.info}}</span></td>
              <td v-else-if="item.info == 'ขาด'"><span class="badge badge-danger">{{item.info}}</span></td>
              <td v-else><span class="badge badge-secondary">{{item.info}}</span></td>
              <td><button type="button" @click="delectObj(item.id)" class="btn btn-danger">ลบ</button></td>
            </tr>
          </tbody>
        </table>
        <footer v-if="showTable"><center>Dev By Teerut</center></footer>
      </div>

    </div>
  </div>
</template>

<script>
  const html2canvas = require('html2canvas');
  export default {
    name: 'App',
    components: {
    },
    data() {
      return {
        dateNow: "",
        timeNow: "",
        dataObj: [],
        form: {
          name: "",
          no: Number,
          info: "",
        },
        selected:0,
        index: 0,
        indexhtml: 0,
        showTable: false,
        nameList: [
          "กิตติศักดิ์ แก้วบัวดี",
          "กฤษกร รุจิราวรรักษ์",
          "ธีรุฒ ศรีทองดี",
          "ณัฐดนัย มูลประเสริฐ",
          "โภคิน ยิ้มแย้ม",
          "คุณาภูมิ สำราญมาก",
          "ธนวัฒน์ มาโต",
          "อชิตะ อินทจร",
          "ณัฐกานต์ ชมจอหอ",
          "พฤฒิพงศ์ ยงสุวัฒนะ",
          "พรหมชนะ เงินบาท",
          "ภูเบศวร์ แดงอร่าม",
          "เสฐียรพงษ์ บริสุทธิ์",
          "พงศกร พวงแก้ว",
          "ทรงพล ศิริรัตน์",
          "ศุภากร แก้วหนองโพธิ์",
          "กฤฎชนก ศรีละมุล",
          "จิรภิภัฐร์ ธนาภัทรชัยสิทธิ์",
          "กรณ์ดนัย สุขกรม",
          "ณัฐวุฒิ ปิ่นแก้ว",
          "ต้นเงิน คล้ายโพธิ์ทอง",
          "ธนภัทร เยี่ยมญาติ",
          "รชานนท์ ประชา",
          "ยศวีร์ เร่งรัด",
          "ฐิติมาภรณ์ คำสี",
          "จิตรานนท์ ใฮวัง",
          "จุฬาลักษณ์ หมดทุกข์",
          "ชญาภรณ์ เพ่งผล",
          "ณัฏฐ์ศศิ แก้วมีศรี",
          "นวฉัตร หว่างดอนไพร",
          "นิลณี อย่างใจนึก",
          "บัณฑิตา กาญจนสาธิตา",
          "ปาริชาติ หอมสง่า",
          "พรนภา กล่ำปี",
          "วรัมพร เบ็ญพาด",
          "ศศิภา เกตุแดง",
          "สาวิตรี ย้อยดำ",
          "อัญชลีพร ชาติตำนาน",
          "เบญญาภา โชคพิรุณทอง",
          "ณัฐชา สดใส",
          "ณัฐณิชา นิยม"
        ],
        result: [],
      }
    },
    methods: {
      addPo() {
        if (this.form.info != "") {
          this.index++
          let Obj = {
            "id": this.index,
            "name": this.form.name,
            "no": this.form.no,
            "info": this.form.info,
          }
          this.dataObj.push(Obj);
          this.resetForm();
        } else {
          this.index++
          let Obj = {
            "id": this.index,
            "name": this.form.name,
            "no": this.form.no,
            "info": "ขาด",
          }
          this.dataObj.push(Obj);
          this.resetForm();
        }
      },
      delectObj(id) {
        this.dataObj = this.dataObj.filter((item) => item.id !== id);
      },
      resetForm() {
        this.form.name = "",
          this.form.no = "",
          this.form.info = ""
      },
      getDateTime() {
        let now = new Date();
        var thday = new Array("อาทิตย์", "จันทร์",
          "อังคาร", "พุธ", "พฤหัส", "ศุกร์", "เสาร์");
        var thmonth = new Array("มกราคม", "กุมภาพันธ์", "มีนาคม",
          "เมษายน", "พฤษภาคม", "มิถุนายน", "กรกฎาคม", "สิงหาคม", "กันยายน",
          "ตุลาคม", "พฤศจิกายน", "ธันวาคม");
        // this.dateNow = date.getHours() + ":" + date.getMinutes() + ":" + date.getSeconds() + " " + date.getDate() +
        //   "/" + date.getMonth() + "/" + date.getFullYear()
        this.dateNow = "วัน" + thday[now.getDay()] + "ที่ " + now.getDate() + " " + thmonth[now.getMonth()] + " " + (0 +
          now.getFullYear() + 543);
        this.timeNow = now.getHours() + ":" + now.getMinutes() + ":" + now.getSeconds();
        this.timeNowSaveFile = now.getHours() + "_" + now.getMinutes() + "_" + now.getSeconds();
      },
      downloadJson(exportObj) {
        let time = Date.now();
        var dataStr = "data:text/json;charset=utf-8," + encodeURIComponent(JSON.stringify(exportObj));
        var downloadAnchorNode = document.createElement('a');
        downloadAnchorNode.setAttribute("href", dataStr);
        downloadAnchorNode.setAttribute("download", time + ".json");
        document.body.appendChild(downloadAnchorNode); // required for firefox
        downloadAnchorNode.click();
        downloadAnchorNode.remove();
      },
      setNumber(no){
        console.log(no);
        this.form.no = no
      },
      searchName(word) {
        for (var i = 0; i < this.nameList.length; i++) {
          if (this.nameList[i].indexof(word) !== -1) {
            let obj = this.nameList[i];
            this.result.push(obj);
          }
        }
      },
      screenCap: function () {
        this.showTable = true
        setTimeout(() => {
          let time = Date.now();
          var offScreen = document.querySelector('#text');
          window.scrollTo(0, 0);
          html2canvas(offScreen, {
            scrollY: -window.scrollY
          }).then((canvas) => {
            var dataURL = canvas.toDataURL("image/png", 1.0);
            var link = document.createElement('a');
            link.href = dataURL;
            link.download = 'นักเรียนที่ลา_' + time + '.png';
            this.imgDataUrl = dataURL;
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
          });
        }, 500);
        setTimeout(() => {
          this.showTable = false
        }, 1000);
      },
    },
    mounted() {
      setInterval(() => {
        this.getDateTime();
      }, 1000)
    },
    created: function () {
      this.getDateTime();
      // fetch("./assets/data.json")
      //   .then(response => {
      //     return response.json();
      //   })
      //   .then(data => console.log(data));
      // var namelist = require('./assets/data.json'); 
      // this.name = namelist;
    }
  }
</script>

<style>
  @import url('https://fonts.googleapis.com/css2?family=Kanit:wght@400&display=swap');

  @font-face {
    font-family: 'THSarabunNew';
    src: url('./assets/THSarabunNew.ttf');
    font-weight: normal;
    font-style: normal;
  }

.paper{
 /* background-image: url(https://receiptify.herokuapp.com/wrinkled-paper-texture-7.jpg); */
}

  body {
    font-family: 'Kanit', sans-serif;
  }

  /* font-family: 'THSarabunNew', sans-serif; */
</style>