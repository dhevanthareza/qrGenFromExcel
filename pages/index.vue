<template>
  <v-col>
    <v-form>
      <v-row>
        <v-col cols="6">
          <v-file-input label="excel" @change="setExcel" />
          <v-btn :loading="loading" color="success" @click="proccess"
            >Proses</v-btn
          >
        </v-col>
      </v-row>
    </v-form>
    <v-row>
      <div v-for="(row, index) in rows.length" :key="index">
        <div :ref="`qrcode${index}`"></div>
      </div>
    </v-row>
  </v-col>
</template>
<script>
import * as QRCode from 'easyqrcodejs'
import readXlsxFile from 'read-excel-file'
// import html2canvas from 'html2canvas'
import logo from './digimedia.png'
// import * as toCanvas from 'vue-html2canvas'
export default {
  data() {
    return {
      excelFile: null,
      rows: [],
      loading: false,
      dataUrls: [],
      isReady: false,
    }
  },
  computed: {
    qrOptions() {
      return {
        title: 'Siapa Kamu',
        config: {
          // text: '1d0OiJpAu9SePQN4ZZkxrF51Lr7eJ-wj4', // Content

          width: 256, // Widht
          height: 256, // Height
          colorDark: '#000000', // Dark color
          colorLight: '#ffffff', // Light color

          quietZone: 15,
          quietZoneColor: '#ffffff',

          // === Title
          // title: 'Chilmi ❤️ Khansa', // Title
          title: 'Chilmi & Khansa', // Title
          titleFont: 'bold 20px Arial', // Title font
          titleColor: '#000000', // Title Color
          titleBackgroundColor: '#fff', // Title Background
          titleHeight: 70, // Title height, include subTitle
          titleTop: 25, // Title draw position(Y coordinate), default is 30

          // === SubTitle
          // subTitle: 'to: Ibu Ummu Hani', // Subtitle content
          subTitleFont: '14px Arial', // Subtitle font
          subTitleColor: '#000000', // Subtitle color
          subTitleTop: 50, // Subtitle drwa position(Y coordinate), default is 50

          // === Logo
          logo, // LOGO

          correctLevel: QRCode.CorrectLevel.H, // L, M, Q, H
        },
      }
    },
  },
  methods: {
    proccess() {
      this.loading = true
      for (let index = 0; index < this.rows.length; index++) {
        const el = this.rows[index]
        if (this.$refs[`qrcode${index}`]) {
          // eslint-disable-next-line no-new
          new QRCode(this.$refs[`qrcode${index}`][0], {
            ...this.qrOptions.config,
            subTitle: el[2],
            text: el[6],
          })
          // const canvas = await html2canvas(this.$refs[`qrcode${index}`][0])
          // console.log(canvas.toDataURL())
          // this.dataUrls.push(canvas.toDataURL())
        }
      }
      // await Promise.all(
      //   this.rows.map((el, index) => {
      //     if (this.$refs[`qrcode${index}`]) {
      //       // eslint-disable-next-line no-new
      //       new QRCode(this.$refs[`qrcode${index}`][0], {
      //         ...this.qrOptions.config,
      //         subTitle: el[2],
      //       })
      //       // const canvas = await html2canvas(this.$refs[`qrcode${index}`][0])
      //       // this.dataUrls.push(canvas.toDataURL())
      //     }
      //     // console.log(this.dataUrls[2])
      //     return el
      //   })
      // )
      this.loading = false
    },
    async setExcel(e) {
      this.loading = true
      const rows = await readXlsxFile(e)
      this.rows = rows
      console.log(this.rows)
      await setTimeout(() => {}, 200)
      console.log(this.$refs.qrcode0)
      console.log(this.$refs)
      this.loading = false
    },
  },
}
</script>
<style scoped>
.container {
  padding: 10px;
}
</style>
