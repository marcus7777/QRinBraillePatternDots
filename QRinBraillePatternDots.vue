<template>
  <div v-if="!bigger" @click="bigger = !bigger" style="display:inline-block;line-height:1;font-weight:bolder;letter-spacing:-.27em;text-shadow:0 0 3px;white-space:pre;outline:black 1px;-webkit-text-stroke-color: black;-webkit-text-stroke:.1em">{{code}}</div>
  <div v-else @click="bigger = !bigger" style="display: inline-block; line-height: 1; font-weight: bolder; letter-spacing: -0.17em; text-shadow: 0px 0px 3px; white-space: pre; outline: black 12px; -webkit-text-stroke: 0.13em; color: #000f; background: #ffff; z-index: 1000; inset: 0px; border: 2px black; position: absolute; width: 100vw; height: 100vw; font-size: calc((8vh + 8vw)/3); min-width: 100vw; text-align: left;">{{code}}</div>
</template>
<script>
  import qr from 'qr.js'
  export default {
    props: {
      value: String,
      level: {
        type: Number,
        default: 0,
      },
    },
    computed: {
      code(){
        const qrcode = qr(this.value, { errorCorrectLevel: this.level })
        const cells = qrcode.modules
        const space = "   \n   "
        let cliQr = space
        for (let r = 0; r < cells.length; r+=4 ) {
          for (let c = 0; c < cells[0].length; c+=2 ) {
            let numBlock = 10240 // blank and start https://www.compart.com/en/unicode/U+2800
            if (cells[r][c])                   numBlock += 1  // 1
            if (cells[r+1] && cells[r+1][c+0]) numBlock += 2  // 2   
            if (cells[r+2] && cells[r+2][c+0]) numBlock += 4  // 3     1 4
            if (cells[r+0] && cells[r+0][c+1]) numBlock += 8  //  4    2 5
            if (cells[r+1] && cells[r+1][c+1]) numBlock += 16 //  5    3 6
            if (cells[r+2] && cells[r+2][c+1]) numBlock += 32 //  6    7 8
            if (cells[r+3] && cells[r+3][c+0]) numBlock += 64 // 7
            if (cells[r+3] && cells[r+3][c+1]) numBlock += 128 // 8
            cliQr += String.fromCodePoint(numBlock)
          }
          cliQr += space
        }        
        return cliQr
      }
    },
    data() {
      return {
        bigger: false,
      }
    },
  }
</script>
